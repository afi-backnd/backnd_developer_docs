---
sidebar_label: NetworkBehaviour
description: "NetworkBehaviour"
---

# NetworkBehaviour

네트워크 기능이 필요한 스크립트의 기본 클래스입니다. MonoBehaviour 대신 상속하면 변수 동기화, RPC 호출, 라이프사이클 콜백을 사용할 수 있습니다. 익숙한 Unity 개발 방식 그대로 멀티플레이어 게임을 만들 수 있습니다.

## 기본 사용법

```csharp
using BACKND;
using UnityEngine;

public class Player : NetworkBehaviour
{
    [SyncVar]
    public int health = 100;

    [Command]
    void CmdTakeDamage(int damage)
    {
        health -= damage;
    }
}
```

## 가상 함수 (Virtual Methods)

NetworkBehaviour 클래스는 네트워크 이벤트에 반응하기 위한 여러 가상 함수를 제공합니다. 이 함수들을 오버라이드하여 네트워크 상태 변화에 따른 로직을 구현할 수 있습니다.

### 가상 함수 목록

| 함수명 | 호출 시점 | 실행 위치 |
|--------|----------|----------|
| `OnStartServer` | 서버에서 오브젝트 스폰 시 | 서버 |
| `OnStopServer` | 서버에서 오브젝트 제거 시 | 서버 |
| `OnStartClient` | 클라이언트에서 오브젝트 스폰 시 | 클라이언트 |
| `OnStopClient` | 클라이언트에서 오브젝트 제거 시 | 클라이언트 |
| `OnStartLocalPlayer` | 로컬 플레이어 오브젝트 스폰 시 | 클라이언트 |
| `OnStopLocalPlayer` | 로컬 플레이어 오브젝트 제거 시 | 클라이언트 |
| `OnStartAuthority` | 소유권 획득 시 | 클라이언트 |
| `OnStopAuthority` | 소유권 상실 시 | 클라이언트 |

### 호출 순서

#### 서버에서 오브젝트 스폰 시
```
1. OnStartServer()
2. OnStartAuthority()    ← 소유자인 경우
3. OnStartClient()       ← 호스트 모드인 경우 (개발/테스트 환경)
4. OnStartLocalPlayer()  ← 로컬 플레이어인 경우
```

#### 클라이언트에서 오브젝트 수신 시
```
1. OnStartClient()
2. OnStartAuthority()    ← 소유자인 경우
3. OnStartLocalPlayer()  ← 로컬 플레이어인 경우
```

#### 오브젝트 제거 시
```
1. OnStopLocalPlayer()   ← 로컬 플레이어인 경우
2. OnStopAuthority()     ← 소유자인 경우
3. OnStopClient()
4. OnStopServer()        ← 서버인 경우
```

---

### OnStartServer

서버에서 오브젝트가 스폰될 때 호출됩니다.

```csharp
public override void OnStartServer()
{
    // 서버에서만 실행되는 초기화 로직
}
```

**주요 용도:**
- 서버 측 초기화 (체력, 스탯 설정)
- AI 로직 시작
- 서버 전용 컴포넌트 활성화
- 매니저/시스템에 등록

```csharp
public class Enemy : NetworkBehaviour
{
    [SyncVar]
    public int health;

    public override void OnStartServer()
    {
        // 서버에서 체력 초기화
        health = 100;

        // AI 시작
        StartCoroutine(AIRoutine());

        // 스폰 매니저에 등록
        EnemyManager.Instance.Register(this);
    }
}
```

---

### OnStopServer

서버에서 오브젝트가 제거될 때 호출됩니다.

```csharp
public override void OnStopServer()
{
    // 서버에서 정리 작업
}
```

**주요 용도:**
- AI/코루틴 정리
- 매니저에서 등록 해제
- 서버 리소스 정리

```csharp
public class Enemy : NetworkBehaviour
{
    public override void OnStopServer()
    {
        // AI 중지
        StopAllCoroutines();

        // 매니저에서 등록 해제
        EnemyManager.Instance.Unregister(this);

        // 드롭 아이템 생성
        SpawnLoot();
    }
}
```

---

### OnStartClient

클라이언트에서 오브젝트가 스폰될 때 호출됩니다. 호스트 모드(개발 환경)의 경우 `OnStartServer` 이후에 호출됩니다.

```csharp
public override void OnStartClient()
{
    // 모든 클라이언트에서 실행되는 초기화 로직
}
```

**주요 용도:**
- 시각적 요소 초기화 (이름표, 외형)
- 오디오 설정
- 클라이언트 측 컴포넌트 활성화
- UI 연결

```csharp
public class Player : NetworkBehaviour
{
    [SyncVar]
    public string playerName;

    public override void OnStartClient()
    {
        // 이름표 설정
        SetupNameplate(playerName);

        // 시각 효과 초기화
        InitializeVisuals();

        // 사운드 초기화
        InitializeAudio();
    }
}
```

---

### OnStopClient

클라이언트에서 오브젝트가 제거될 때 호출됩니다.

```csharp
public override void OnStopClient()
{
    // 클라이언트 정리 작업
}
```

**주요 용도:**
- 시각 효과 정리
- UI 요소 제거
- 클라이언트 리소스 해제
- 이벤트 구독 해제

```csharp
public class Player : NetworkBehaviour
{
    public override void OnStopClient()
    {
        // 이름표 제거
        RemoveNameplate();

        // 이펙트 정리
        CleanupEffects();

        // UI에서 제거
        PlayerListUI.Instance.RemovePlayer(this);
    }
}
```

---

### OnStartLocalPlayer

로컬 플레이어 오브젝트가 스폰될 때 호출됩니다. 자신이 조종하는 플레이어 오브젝트에서만 호출됩니다.

```csharp
public override void OnStartLocalPlayer()
{
    // 로컬 플레이어만 실행하는 초기화
}
```

**주요 용도:**
- 카메라 설정
- 입력 시스템 활성화
- 플레이어 전용 UI 표시
- 로컬 전용 컴포넌트 활성화

```csharp
public class PlayerController : NetworkBehaviour
{
    public override void OnStartLocalPlayer()
    {
        // 카메라 연결
        Camera.main.GetComponent<CameraFollow>().SetTarget(transform);

        // 입력 시스템 활성화
        GetComponent<PlayerInput>().enabled = true;

        // 플레이어 HUD 표시
        PlayerHUD.Instance.Show();

        // 미니맵 아이콘 설정
        Minimap.Instance.SetPlayerMarker(transform);
    }
}
```

---

### OnStopLocalPlayer

로컬 플레이어 오브젝트가 제거될 때 호출됩니다.

```csharp
public override void OnStopLocalPlayer()
{
    // 로컬 플레이어 정리 작업
}
```

**주요 용도:**
- 카메라 분리
- 입력 비활성화
- 플레이어 UI 숨김
- 로컬 리소스 정리

```csharp
public class PlayerController : NetworkBehaviour
{
    public override void OnStopLocalPlayer()
    {
        // 카메라 분리
        Camera.main.GetComponent<CameraFollow>().ClearTarget();

        // 입력 비활성화
        GetComponent<PlayerInput>().enabled = false;

        // HUD 숨김
        PlayerHUD.Instance.Hide();
    }
}
```

---

### OnStartAuthority

클라이언트가 오브젝트의 소유권(Authority)을 획득할 때 호출됩니다.

:::tip 소유권이란?
소유권은 클라이언트가 서버를 거치지 않고 **오브젝트를 직접 조작**할 수 있게 해주는 기능입니다.
레이싱 게임의 차량처럼 **즉각적인 반응이 필수**인 경우에만 사용합니다.
대부분의 게임 로직은 Command로 충분합니다. 자세한 내용은 [NetworkIdentity - 소유권 시스템](./network-identity#소유권authority-시스템)을 참고하세요.
:::

```csharp
public override void OnStartAuthority()
{
    // 소유권 획득 시 실행
}
```

**주요 용도:**
- 직접 조작 입력 활성화
- 전용 카메라 전환
- 조작 UI 표시

```csharp
public class Vehicle : NetworkBehaviour
{
    public override void OnStartAuthority()
    {
        // 조작 입력 활성화 - 이제 이 클라이언트가 직접 차량을 움직임
        GetComponent<VehicleInput>().enabled = true;

        // 카메라 전환
        VehicleCamera.Instance.SetTarget(transform);
    }
}
```

---

### OnStopAuthority

클라이언트가 오브젝트의 소유권을 잃을 때 호출됩니다.

```csharp
public override void OnStopAuthority()
{
    // 소유권 상실 시 실행
}
```

**주요 용도:**
- 조작 입력 비활성화
- 카메라 복원
- 조작 UI 숨김

```csharp
public class Vehicle : NetworkBehaviour
{
    public override void OnStopAuthority()
    {
        // 조작 입력 비활성화
        GetComponent<VehicleInput>().enabled = false;

        // 카메라 복원
        VehicleCamera.Instance.ClearTarget();
    }
}
```

## 상태 확인 프로퍼티

```csharp
public class GameLogic : NetworkBehaviour
{
    void Update()
    {
        // 서버에서 실행 중인지
        if (isServer) { }

        // 클라이언트에서 실행 중인지
        if (isClient) { }

        // 이 클라이언트가 소유한 오브젝트인지
        if (isOwned) { }

        // 로컬 플레이어인지
        if (isLocalPlayer) { }

        // 호스트 모드인지 (개발/테스트 환경에서만 true)
        if (isServer && isClient) { }
    }
}
```

## 실전 예시

```csharp
public class Player : NetworkBehaviour
{
    [SyncVar(hook = nameof(OnHealthChanged))]
    public int health = 100;

    [SyncVar]
    public string playerName;

    // 서버: 초기화
    public override void OnStartServer()
    {
        health = 100;
    }

    // 클라이언트: 시각 요소 설정
    public override void OnStartClient()
    {
        SetupNameplate(playerName);
    }

    // 로컬 플레이어: 입력/카메라 활성화
    public override void OnStartLocalPlayer()
    {
        Camera.main.GetComponent<CameraFollow>().SetTarget(transform);
        GetComponent<PlayerInput>().enabled = true;
    }

    // 클라이언트 → 서버 요청
    [Command]
    public void CmdUseSkill(int skillId, Vector3 targetPos)
    {
        // 서버에서 스킬 처리
        SkillManager.Instance.ExecuteSkill(this, skillId, targetPos);
    }

    // 서버 → 모든 클라이언트 알림
    [ClientRpc]
    public void RpcPlaySkillEffect(int skillId)
    {
        EffectManager.Instance.PlaySkill(skillId, transform.position);
    }

    void OnHealthChanged(int oldHealth, int newHealth)
    {
        UpdateHealthUI(newHealth);
    }
}
```

### 코스튬 동기화

접속 시 저장된 외형 데이터를 로드하고 모든 클라이언트에 동기화하는 예시입니다.

```csharp
public class PlayerAppearance : NetworkBehaviour
{
    [SyncVar(hook = nameof(OnCostumeChanged))]
    public int costumeId;

    [SyncVar(hook = nameof(OnHairChanged))]
    public int hairStyleId;

    [SyncVar(hook = nameof(OnColorChanged))]
    public Color hairColor;

    public override void OnStartServer()
    {
        // 서버: DB에서 외형 데이터 로드
        var data = Database.LoadAppearance(connectionToClient);
        costumeId = data.costumeId;
        hairStyleId = data.hairStyleId;
        hairColor = data.hairColor;
    }

    public override void OnStartClient()
    {
        // 클라이언트: 현재 SyncVar 값으로 외형 적용
        ApplyCostume(costumeId);
        ApplyHairStyle(hairStyleId);
        ApplyHairColor(hairColor);
    }

    // SyncVar 변경 시 실시간 반영
    void OnCostumeChanged(int oldId, int newId) => ApplyCostume(newId);
    void OnHairChanged(int oldId, int newId) => ApplyHairStyle(newId);
    void OnColorChanged(Color oldColor, Color newColor) => ApplyHairColor(newColor);

    void ApplyCostume(int id)
    {
        // 코스튬 메시/머티리얼 변경
        var costume = CostumeDatabase.Get(id);
        skinnedMeshRenderer.sharedMesh = costume.mesh;
        skinnedMeshRenderer.material = costume.material;
    }

    void ApplyHairStyle(int id) { /* 헤어 메시 변경 */ }
    void ApplyHairColor(Color color) { /* 헤어 색상 변경 */ }
}
```

:::note
1. NetworkBehaviour를 사용하려면 같은 GameObject에 NetworkIdentity가 있어야 합니다
2. Start()보다 OnStartServer/OnStartClient를 사용하여 네트워크 초기화를 수행하세요
3. Update()에서 네트워크 상태(isServer, isClient, isLocalPlayer)를 확인하여 적절한 로직만 실행하세요
4. 서버에서만 실행해야 하는 로직은 [Server] 어트리뷰트나 isServer 체크를 사용하세요
5. 한 오브젝트에 최대 64개의 NetworkBehaviour만 허용됩니다
:::
