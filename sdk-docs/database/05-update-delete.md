---
sidebar_label: 데이터 수정 및 삭제
---

# 데이터 수정 및 삭제

데이터베이스에 저장된 데이터를 수정하거나 삭제하는 방법을 설명합니다.

:::warning 성능 주의
`Where` 절을 사용하여 대상을 특정할 때, **인덱스(Index)**나 **기본 키(PrimaryKey)**를 사용하지 않으면 테이블 전체를 탐색하는 **풀 스캔(Full Scan)** 방식으로 처리됩니다.
데이터가 많아질수록 작업 속도가 느려질 수 있으므로 주의가 필요합니다. [인덱스 생성 가이드](/guide/console-guide/database/table-editor/#테이블-수정)
:::

## 데이터 수정 (Update)

데이터를 수정하려면 먼저 대상을 특정한 후 `Update` 메서드를 호출해야 합니다.

### 단일 객체 수정

일반적으로 데이터를 조회한 후, 값을 변경하고 다시 저장하는 방식을 사용합니다.

```csharp
// 1. 수정할 유저 조회
var user = await DBClient.From<User>()
    .Where(x => x.Username == "user_001")
    .FirstOrDefault();

if (user != null)
{
    // 2. 값 변경
    user.Level = 5;
    user.Experience = 1000;
    user.LastLogin = DateTime.UtcNow;

    // 3. 변경된 내용 저장 (ID를 기준으로 업데이트)
    var result = await DBClient.From<User>()
        .Where(x => x.Id == user.Id)
        .Update(user);

    Debug.Log($"업데이트 완료: {result.AffectedRows}행 영향");
}
```

### 특정 필드만 수정

특정 필드만 수정하려면, 수정할 필드만 `Set` 메서드로 지정할 수 있습니다.

```csharp
// 특정 필드만 수정
var result = await DBClient.From<User>()
    .Where(x => x.Id == targetId)
    .Set(x => x.Level, 10)
    .Set(x => x.Experience, 5000)
    .Update();

Debug.Log($"업데이트 완료: {result.AffectedRows}행 영향");
```

### 조건부 일괄 수정

특정 조건에 맞는 데이터를 한 번에 수정할 수도 있습니다.

```csharp
// 비활성 유저를 모두 활성화 상태로 변경
var updateTemplate = new User { IsActive = true };

var result = await DBClient.From<User>()
    .Where(x => x.IsActive == false)
    .Update(updateTemplate); // 주의: 설정된 필드만 업데이트됩니다.

Debug.Log($"{result.AffectedRows}명의 유저가 활성화되었습니다.");
```

## MutationResult

`Update()`, `Delete()` 메서드는 `MutationResult` 객체를 반환합니다.

| 속성 | 타입 | 설명 |
|------|------|------|
| `AffectedRows` | `int` | 영향받은 행 수 |
| `Operation` | `string` | 실행된 작업 타입 ("UPDATE", "DELETE") |
| `Message` | `string` | 결과 메시지 |

```csharp
var result = await DBClient.From<User>()
    .Where(x => x.Id == targetId)
    .Update(user);

if (result.AffectedRows > 0)
{
    Debug.Log("업데이트 성공!");
}
else
{
    Debug.Log("업데이트할 데이터가 없습니다.");
}
```

## 산술 연산자 (Inc / Dec)

데이터를 조회하지 않고 직접 숫자 필드를 증가(Increment)하거나 감소(Decrement)할 수 있습니다.  

### 언제 사용하나요?

- **카운터/누적치 갱신**
- **소모/차감 처리**
- **동시성 환경**

`Update()`로 값을 직접 덮어쓰는 방식은 보통 **조회 → 값 계산 → 저장** 흐름이 필요합니다. 반면 `Inc()`/`Dec()`는 서버에서 **증감 연산을 바로 적용**하므로, 값 덮어쓰기 방식보다 충돌 가능성을 줄일 수 있습니다.

:::tip 핵심 포인트
- **조회 없이 증감**: 네트워크/처리 비용 감소
- **원자적(Atomic) 처리**: 한 번의 실행으로 여러 필드를 함께 갱신
:::

### 예시: 점수 및 전적 반영 (Inc / Dec)

```csharp
[Table("battle_record", TableType.UserTable)]
public class BattleRecord : BaseModel
{
    [PrimaryKey]
    [Column(NotNull = true)]
    public string InDate { get; set; } // 유저 식별자 (inDate)

    [Column(NotNull = true, DefaultValue = "1000")]
    public int MMR { get; set; } = 1000;

    [Column(NotNull = true, DefaultValue = "0")]
    public int Wins { get; set; } = 0;

    [Column(NotNull = true, DefaultValue = "0")]
    public int Losses { get; set; } = 0;
}

// 승리 처리 예시: 내 점수/승리 증가, 상대 점수/패배 증가
await DBClient.From<BattleRecord>()
    .Where(x => x.InDate == myInDate)
    .Inc(x => x.MMR, scoreChange)
    .Inc(x => x.Wins, 1)
    .Update();

await DBClient.From<BattleRecord>()
    .Where(x => x.InDate == opponentInDate)
    .Dec(x => x.MMR, scoreChange)
    .Inc(x => x.Losses, 1)
    .Update();
```

### 사용 규칙 및 주의사항

- **숫자 필드만 지원**: `int`, `long`, `float`, `double` 등 숫자 타입 컬럼에만 사용하세요.
- **반드시 `Update()` 호출**: `Inc()`/`Dec()`를 체이닝한 뒤 `Update()`를 호출해야 실제로 반영됩니다.

## 데이터 삭제 (Delete)

`Delete` 메서드를 사용하여 조건에 맞는 데이터를 삭제합니다.

### 단일 삭제

```csharp
// ID가 특정 값인 유저 삭제
var result = await DBClient.From<User>()
    .Where(x => x.Id == targetId)
    .Delete();

Debug.Log($"삭제 완료: {result.AffectedRows}행 영향");
```

### 조건부 일괄 삭제

```csharp
// 레벨이 1 미만인 모든 유저 삭제
var result = await DBClient.From<User>()
    .Where(x => x.Level < 1)
    .Delete();

Debug.Log($"{result.AffectedRows}명의 유저가 삭제되었습니다.");
```
