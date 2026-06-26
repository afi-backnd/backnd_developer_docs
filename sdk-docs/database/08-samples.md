---
sidebar_label: 구현 예제
description: "구현 예제"
---

# 구현 예제

데이터베이스를 활용하여 실제 서비스에서 자주 사용되는 시스템을 구현하는 예제입니다.

## 1. 공유 데이터 관리 (예: 월드 보스)

전체 유저가 공유하는 데이터를 관리하고, 개별 유저의 기여도를 기록하는 시스템입니다.
(예: 월드 보스 레이드, 서버 전체 목표 달성 이벤트 등)

### 데이터 모델

```csharp
[Table("world_boss", TableType.FlexibleTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.OTHERS }, WritePermissions = new[] { TablePermission.OTHERS })]
public class WorldBoss : BaseModel
{
    [PrimaryKey(AutoIncrement = true)]
    [Column(NotNull = true)]
    public int Id { get; set; }

    [Column(NotNull = true, DefaultValue = "0")]
    public long TotalDamage { get; set; } = 0; // 누적 데미지
}

[Table("boss_raid_record", TableType.UserTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.SELF }, WritePermissions = new[] { TablePermission.SELF })]
public class RaidRecord : BaseModel
{
    [PrimaryKey]
    [Column(NotNull = true)]
    public string InDate { get; set; } // 유저 식별자 (inDate)

    [Column(NotNull = true)]
    public int BossId { get; set; }

    [Column(NotNull = true, DefaultValue = "0")]
    public long TotalDamage { get; set; } = 0;

    [Column]
    public DateTime? LastAttackTime { get; set; }
}
```

### 데이터 조회 및 갱신

:::tip 성능 팁
`Where` 절에서 `BossId`로 조회하는 경우, 해당 컬럼에 인덱스를 설정하면 조회 성능이 향상됩니다. [인덱스 생성 가이드](/guide/console-guide/database/table-editor/#테이블-수정)
:::

```csharp
public async Task AttackBoss(int bossId, long damage)
{
    // 1. 차트(정적 데이터)에서 보스 정보(MaxHp) 조회
    long maxHp = 100000; 

    // 2. 현재 보스 상태 조회 (누적 데미지)
    var boss = await DBClient.From<WorldBoss>()
        .Where(b => b.Id == bossId)
        .FirstOrDefault();

    if (boss == null)
    {
        // 보스 정보가 없으면 새로 생성
        boss = new WorldBoss
        {
            Id = bossId,
            TotalDamage = 0
        };
        await DBClient.From<WorldBoss>().Insert(boss);
    }

    // 3. 데미지 계산
    long validDamage = damage;
    if (boss.TotalDamage + damage > maxHp)
    {
        validDamage = maxHp - boss.TotalDamage;
    }

    if (validDamage <= 0) return; // 이미 처치됨

    // 4. 보스 누적 데미지 업데이트
    var result = await DBClient.From<WorldBoss>()
        .Where(b => b.Id == bossId)
        .Inc(b => b.TotalDamage, validDamage)
        .Update();

    if (result.AffectedRows == 0) return; // 동시성 처리: 이미 처치된 경우

    // 5. 기여도(데미지) 기록 업데이트
    var myRecord = await DBClient.From<RaidRecord>()
        .Where(r => r.BossId == bossId)
        .FirstOrDefault();

    if (myRecord == null)
    {
        // 첫 기록 생성
        myRecord = new RaidRecord
        {
            BossId = bossId,
            TotalDamage = validDamage,
            LastAttackTime = DateTime.UtcNow
        };
        await DBClient.From<RaidRecord>().Insert(myRecord);
    }
    else
    {
        // 기록 갱신
        myRecord.TotalDamage += validDamage;
        myRecord.LastAttackTime = DateTime.UtcNow;
        await DBClient.From<RaidRecord>()
            .Where(r => r.InDate == myRecord.InDate)
            .Update(myRecord);
    }
}
```

## 2. 점수 및 랭킹 시스템 (예: ELO 레이팅)

유저 간의 경쟁 결과에 따라 점수를 계산하고 갱신하는 시스템입니다.
이 예제는 **ELO Rating System** 공식을 적용하여, 실력 차이에 따른 공정한 점수 변동을 구현합니다.

### 데이터 모델

```csharp
[Table("battle_record", TableType.UserTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.SELF, TablePermission.OTHERS }, WritePermissions = new[] { TablePermission.SELF, TablePermission.OTHERS })]
public class BattleRecord : BaseModel
{
    [PrimaryKey]
    [Column(NotNull = true)]
    public string InDate { get; set; } // 유저 식별자 (inDate)

    [Column]
    public string Nickname { get; set; }

    [Column(NotNull = true, DefaultValue = "1000")]
    public int MMR { get; set; } = 1000; // 기본 점수

    [Column(NotNull = true, DefaultValue = "0")]
    public int Wins { get; set; } = 0;

    [Column(NotNull = true, DefaultValue = "0")]
    public int Losses { get; set; } = 0;
}
```

### 점수 계산 및 결과 처리

```csharp
public class EloSystem
{
    // ELO 변동폭 계산 (K-Factor: 가중치, 보통 32 사용)
    private int CalculateEloChange(int myRating, int opponentRating, bool isWin, int kFactor = 32)
    {
        // 승리 기대 확률 계산
        double expectedScore = 1.0 / (1.0 + Math.Pow(10.0, (opponentRating - myRating) / 400.0));
        
        // 실제 결과 (승리: 1.0, 패배: 0.0)
        double actualScore = isWin ? 1.0 : 0.0;
        
        // 변동 점수 계산
        return (int)(kFactor * (actualScore - expectedScore));
    }

    // 매치 결과 처리 함수
    public async Task ProcessMatchResult(string myInDate, string opponentInDate, bool isWin)
    {
        // 1. 두 유저의 정보 조회
        var me = await DBClient.From<BattleRecord>()
            .Where(x => x.InDate == myInDate)
            .FirstOrDefault();

        var opponent = await DBClient.From<BattleRecord>()
            .Where(x => x.InDate == opponentInDate)
            .FirstOrDefault();

        if (me == null || opponent == null) return;

        // 2. 점수 변동 계산
        int scoreChange = CalculateEloChange(me.MMR, opponent.MMR, isWin);

        // 3. 점수 및 전적 반영 (Inc/Dec 사용)
        if (isWin)
        {
            // 승리: 내 MMR 증가 + 승리 횟수 증가
            await DBClient.From<BattleRecord>()
                .Where(x => x.InDate == me.InDate)
                .Inc(x => x.MMR, scoreChange)
                .Inc(x => x.Wins, 1)
                .Update();

            // 패배: 상대 MMR 감소 + 패배 횟수 증가
            await DBClient.From<BattleRecord>()
                .Where(x => x.InDate == opponent.InDate)
                .Dec(x => x.MMR, scoreChange)
                .Inc(x => x.Losses, 1)
                .Update();
        }
        else
        {
            // 패배: 내 MMR 감소 + 패배 횟수 증가
            await DBClient.From<BattleRecord>()
                .Where(x => x.InDate == me.InDate)
                .Dec(x => x.MMR, scoreChange)
                .Inc(x => x.Losses, 1)
                .Update();

            // 승리: 상대 MMR 증가 + 승리 횟수 증가
            await DBClient.From<BattleRecord>()
                .Where(x => x.InDate == opponent.InDate)
                .Inc(x => x.MMR, scoreChange)
                .Inc(x => x.Wins, 1)
                .Update();
        }
    }
}
```

## 3. 리더보드 연동

데이터베이스와 리더보드를 연동하여 점수를 업데이트하면서 동시에 리더보드를 갱신하는 예제입니다.

:::info 리더보드란?
뒤끝 베이스에서 제공하는 순위 기능입니다. 데이터베이스의 테이블과 연결하여 점수 기반 순위를 관리할 수 있습니다.
자세한 내용은 [리더보드 문서](/sdk-docs/backend/base/leaderboard/user/update)를 참고하세요.
:::

:::warning 데이터베이스 연동 주의사항
데이터베이스를 사용하여 리더보드와 연동할 경우, 반드시 베이스 SDK 버전이 **5.18.5** 이상인지 확인해주시기 바랍니다.  
데이터베이스 버전이 **5.18.5** 미만일 경우, 리더보드 갱신이 정상적으로 이루어지지 않을 수 있습니다.  
:::

### 데이터 모델

```csharp
[Table("boss_leaderboard", TableType.UserTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.SELF }, WritePermissions = new[] { TablePermission.SELF })]
public class BossLeaderboard : BaseModel
{
    [PrimaryKey]
    [Column(NotNull = true)]
    public string Indate { get; set; } // 유저 식별자 (inDate)

    [Column(NotNull = true, DefaultValue = "0")]
    public int Score { get; set; } = 0;
}
```

### 점수 업데이트 및 리더보드 갱신

```csharp
public async Task UpdateBossLeaderboard()
{
    // 1. 내 리더보드 데이터 조회
    var row = await DBClient.From<BossLeaderboard>().ToList();

    if (row.Count == 0)
    {
        // 2-1. 데이터가 없으면 새로 삽입
        var leaderboardEntry = new BossLeaderboard
        {
            Indate = Backend.UserInDate,
            Score = 0
        };

        await DBClient.From<BossLeaderboard>().Insert(leaderboardEntry);
    }
    else
    {
        // 2-2. 데이터가 있으면 점수 업데이트 + 리더보드 갱신
        row[0].Score += 100;

        Param param = new Param();
        param.Add("score", row[0].Score);

        // 리더보드 갱신 (데이터베이스 업데이트 + 리더보드 순위 반영)
        var bro = Backend.Leaderboard.User.UpdateMyDataAndRefreshLeaderboard(
            "리더보드 uuid",       // 뒤끝 콘솔에서 생성한 리더보드의 uuid
            "boss_leaderboard",   // 테이블 이름
            row[0].GetPrimaryKey(), // 업데이트할 row의 inDate (PrimaryKey)
            param
        );
    }
}
```

:::tip 핵심 포인트
- **UpdateMyDataAndRefreshLeaderboard**: 테이블 데이터 업데이트와 리더보드 갱신을 한 번에 처리합니다.
- **리더보드 uuid**: 뒤끝 콘솔에서 리더보드 생성 후 확인하거나, [모든 유저 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/user/get-leaderboard) 함수로 확인할 수 있습니다.
- **사전 조건**: 리더보드를 갱신하려면 해당 테이블에 먼저 1회 Insert하여 row를 생성해야 합니다.
:::

## 4. 진행도 관리 (예: 퀘스트)

유저별 작업 진행 상황을 저장하고 관리합니다.
(예: 퀘스트, 업적, 일일 미션 등)

### 데이터 모델

```csharp
[Table("quest_progress", TableType.UserTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.SELF }, WritePermissions = new[] { TablePermission.SELF })]
public class QuestProgress : BaseModel
{
    [PrimaryKey(AutoIncrement = true)]
    [Column(NotNull = true)]
    public long Id { get; set; }

    [Column]
    public int? QuestId { get; set; }

    [Column(NotNull = true, DefaultValue = "0")]
    public int CurrentCount { get; set; } = 0; // 진행도

    [Column(NotNull = true, DefaultValue = "0")]
    public int TargetCount { get; set; } = 0;  // 목표치

    [Column(NotNull = true, DefaultValue = "false")]
    public bool IsCompleted { get; set; } = false;
    
    [Column(NotNull = true, DefaultValue = "false")]
    public bool IsRewardClaimed { get; set; } = false;
}
```

### 진행도 업데이트

:::tip 성능 팁
자주 조회하는 `QuestId`에 인덱스를 설정하면 조회 성능이 향상됩니다. [인덱스 생성 가이드](/guide/console-guide/database/table-editor/#테이블-수정)
:::

```csharp
public async Task UpdateQuestProgress(int questId, int addCount)
{
    // 내 퀘스트 정보 조회
    var quest = await DBClient.From<QuestProgress>()
        .Where(q => q.QuestId == questId)
        .FirstOrDefault();

    if (quest != null && !quest.IsCompleted)
    {
        // 진행도 증가
        quest.CurrentCount += addCount;

        // 목표 달성 시 완료 처리
        if (quest.CurrentCount >= quest.TargetCount)
        {
            quest.CurrentCount = quest.TargetCount;
            quest.IsCompleted = true;
        }

        await DBClient.From<QuestProgress>()
            .Where(q => q.Id == quest.Id)
            .Update(quest);
    }
}
```
