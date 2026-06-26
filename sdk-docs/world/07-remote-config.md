---
sidebar_label: 리모트 컨피그
description: "리모트 컨피그"
---

# Remote Config

게임 설정값을 서버에서 실시간으로 관리합니다. 콘솔에서 설정을 변경하면 모든 클라이언트에 즉시 반영됩니다.

앱 업데이트 없이 게임 밸런스, 이벤트, 시스템 설정을 변경할 수 있습니다.

## 주요 기능
- 실시간 설정값 변경 및 적용
- 모든 클라이언트 자동 동기화
- 게임 밸런스 실시간 조정
- 이벤트 및 시스템 설정 관리
- 다양한 데이터 타입 지원

## 설정 방법

### 1. 컴포넌트 추가
NetworkManager를 Scene에 추가하면 자동으로 RemoteConfig 컴포넌트가 포함됩니다.

![Remote Config Component](/img/docs/guide/world/sdk/remote-config-01.png)

### 2. Inspector에서 설정
Inspector 창에서 다음 기능들을 사용할 수 있습니다.

![Remote Config Inspector](/img/docs/guide/world/sdk/remote-config-02.png)

- **Load**: 현재 서버의 설정값을 불러옵니다
- **Save**: 변경한 설정값을 서버에 저장합니다
- **Add New Value**: 새로운 설정값을 추가합니다
- **X**: 선택한 설정값을 삭제합니다

## 데이터 타입

### 기본 타입
```csharp
bool        // true/false
int         // 32비트 정수
uint        // 부호 없는 32비트 정수
int64       // 64비트 정수
uint64      // 부호 없는 64비트 정수
float       // 부동 소수점
string      // 문자열
DateTime    // 날짜/시간
```

### JSON (커스텀 클래스)
복잡한 데이터는 사용자 정의 클래스를 만들어 JSON 형식으로 저장할 수 있습니다.

## 사용 예시

### 1. 기본적인 설정값 사용
```csharp
public class GameManager : MonoBehaviour
{
    private void Start()
    {
        var config = NetworkManager.Instance.RemoteConfig;

        // 시스템 설정
        bool maintenance = config.GetValue<bool>("maintenanceMode");
        int maxUsers = config.GetValue<int>("maxConcurrentUsers");
        float dropRate = config.GetValue<float>("rareItemDropRate");
        
        // 플레이어 설정
        float moveSpeed = config.GetValue<float>("playerMoveSpeed");
        int startHealth = config.GetValue<int>("playerStartHealth");
        
        // 이벤트 설정
        DateTime eventEnd = config.GetValue<DateTime>("eventEndTime");
        string eventMessage = config.GetValue<string>("eventMessage");
    }
}
```

### 2. JSON 설정값 사용
```csharp
// 스폰 위치 정보를 위한 클래스
[System.Serializable]
public class SpawnLocation
{
    public float x;
    public float y;
    public float z;
}

// 몬스터 설정을 위한 클래스
[System.Serializable]
public class MonsterSettings
{
    public int health;
    public float damage;
    public float respawnTime;
    public SpawnLocation[] spawnPoints;
}

public class MonsterManager : MonoBehaviour
{
    private void Start()
    {
        var config = NetworkManager.Instance.RemoteConfig;

        // 단일 위치 정보 가져오기
        var playerSpawn = config.GetValue<SpawnLocation>("defaultSpawnLocation");
        transform.position = new Vector3(playerSpawn.x, playerSpawn.y, playerSpawn.z);

        // 복잡한 설정 정보 가져오기
        var bossSettings = config.GetValue<MonsterSettings>("bossMonsterSettings");
        SetupBossMonster(bossSettings);
    }
}
```

### 3. 이벤트 시간 관리
```csharp
public class EventManager : MonoBehaviour
{
    private void Start()
    {
        var config = NetworkManager.Instance.RemoteConfig;
        DateTime eventStart = config.GetValue<DateTime>("eventStartTime");
        DateTime eventEnd = config.GetValue<DateTime>("eventEndTime");

        // 이벤트 기간 확인
        bool isEventPeriod = DateTime.Now >= eventStart && DateTime.Now <= eventEnd;
        
        // 남은 시간 계산
        if (isEventPeriod)
        {
            TimeSpan remaining = eventEnd - DateTime.Now;
            Debug.Log($"이벤트 종료까지 {remaining.Days}일 {remaining.Hours}시간");
        }
    }
}
```

## 실전 활용 예시

### 게임 밸런스 조정
```csharp
public class BalanceManager : MonoBehaviour
{
    // 밸런스 값들을 Remote Config로 관리
    private void UpdateBalance()
    {
        var config = NetworkManager.Instance.RemoteConfig;
        
        GameBalance.DamageMultiplier = config.GetValue<float>("damageMultiplier");
        GameBalance.ExpMultiplier = config.GetValue<float>("expMultiplier");
        GameBalance.DropRateMultiplier = config.GetValue<float>("dropRateMultiplier");
        GameBalance.GoldMultiplier = config.GetValue<float>("goldMultiplier");
    }
}
```

### 시스템 상태 관리
```csharp
public class SystemManager : MonoBehaviour
{
    private void CheckSystemStatus()
    {
        var config = NetworkManager.Instance.RemoteConfig;
        
        // 서버 상태 확인
        bool maintenance = config.GetValue<bool>("maintenanceMode");
        int maxUsers = config.GetValue<int>("maxConcurrentUsers");
        int latencyLimit = config.GetValue<int>("regionLatencyLimit");

        if (maintenance)
        {
            string notice = config.GetValue<string>("maintenanceNotice");
            ShowMaintenancePopup(notice);
        }
    }
}
```

:::tip 활용 팁
1. **게임 밸런싱**
   - 무기 데미지, 경험치량, 드롭률 등 밸런스 요소
   - 난이도 조절값
   - 보상 배율

2. **이벤트 관리**
   - 이벤트 시작/종료 시간
   - 이벤트 보상 및 확률
   - 이벤트 메시지

3. **시스템 관리**
   - 서버 점검 모드
   - 동시접속자 제한
   - 지역별 설정

4. **콘텐츠 관리**
   - 공지사항
   - 튜토리얼 설정
:::

:::note 주의사항
- 값을 가져올 때는 반드시 올바른 타입을 사용하세요
- JSON 데이터는 직렬화 가능하도록 [System.Serializable] 속성을 지정하세요
- 중요한 보안 정보는 Remote Config에 저장하지 마세요
- 게임의 핵심 로직을 Remote Config에 의존하지 않도록 설계하세요
:::
