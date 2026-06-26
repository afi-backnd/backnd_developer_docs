---
sidebar_label: NetworkIdentity
description: "NetworkIdentity"
---

# NetworkIdentity

모든 네트워크 오브젝트의 필수 컴포넌트입니다. 네트워크 ID 할당, 소유권 관리, 관찰자 시스템이 내장되어 있습니다.

## 필수 요구사항

:::important
모든 네트워크 오브젝트에는 반드시 NetworkIdentity 컴포넌트가 있어야 합니다:
- 플레이어 프리팹
- 스폰되는 모든 오브젝트
- 씬에 배치된 네트워크 오브젝트
:::

## 주요 속성

### 상태 확인 프로퍼티

```csharp
public class MyNetworkObject : NetworkBehaviour
{
    void Update()
    {
        // 서버에서 실행 중인지 확인
        if (isServer) { /* 서버 로직 */ }

        // 클라이언트에서 실행 중인지 확인
        if (isClient) { /* 클라이언트 로직 */ }

        // 로컬 플레이어인지 확인
        if (isLocalPlayer) { /* 로컬 플레이어 전용 로직 */ }

        // 이 클라이언트가 소유권을 가지고 있는지 확인
        if (isOwned) { /* 소유자 전용 로직 */ }

        // 서버 전용 (클라이언트 아님)
        if (isServerOnly) { /* 서버 전용 로직 */ }

        // 클라이언트 전용 (서버 아님)
        if (isClientOnly) { /* 클라이언트 전용 로직 */ }
    }
}
```

### 네트워크 ID

```csharp
// 네트워크에서 오브젝트를 고유하게 식별하는 ID
ulong objectNetId = netIdentity.netId;

// 에셋(프리팹) 식별자
uint assetId = netIdentity.assetId;

// 씬 오브젝트 식별자
ulong sceneId = netIdentity.sceneId;
```

### 연결 정보

```csharp
// 서버로의 연결 (클라이언트에서 사용)
NetworkConnection serverConnection = netIdentity.connectionToServer;

// 소유 클라이언트로의 연결 (서버에서 사용)
NetworkConnectionToClient ownerConnection = netIdentity.connectionToClient;

// 이 오브젝트를 관찰 중인 모든 클라이언트
Dictionary<uint, NetworkConnectionToClient> allObservers = netIdentity.observers;
```

## Inspector 설정

### Server Only
- `true`: 이 오브젝트는 서버에서만 존재하며 클라이언트에 동기화되지 않습니다
- `false` (기본값): 클라이언트에도 동기화됩니다

```csharp
// 코드에서 확인
if (netIdentity.serverOnly)
{
    // 이 오브젝트는 서버에서만 존재
}
```

## 소유권(Authority) 시스템

### 소유권이란?

소유권은 **클라이언트가 특정 오브젝트를 직접 조작**할 수 있게 해주는 기능입니다.

일반적인 네트워크 흐름:
```
클라이언트 → [Command] → 서버가 처리 → [SyncVar/RPC] → 클라이언트에 반영
```

소유권이 있을 때의 흐름:
```
클라이언트가 직접 조작 → 서버에 자동 전송 → 다른 클라이언트에 반영
```

### 언제 필요한가?

**대부분의 경우 Command로 충분합니다.** 소유권은 다음 상황에서만 필요합니다:

| 상황 | 권장 방식 | 이유 |
|------|----------|------|
| 아이템 획득, 스킬 사용 | Command | 서버 검증 필요 |
| 일반 캐릭터 이동 | Command | 서버 왕복 지연이 허용됨 |
| **레이싱 게임 차량 조작** | 소유권 | 즉각적인 반응 필수 |
| **FPS 에임/시점** | 소유권 | 50ms 지연도 치명적 |

:::warning 핵심 제약
**한 오브젝트는 한 번에 한 클라이언트만 소유**할 수 있습니다.
- 10명이 같은 차량을 조종하려 해도, 서버가 1명만 선택
- 나머지는 거절되거나 대기열에 등록
:::

### 사용 예시: 차량 탑승 시스템

```csharp
public class Vehicle : NetworkBehaviour
{
    [SyncVar]
    public bool isOccupied;

    // 클라이언트: 탑승 요청
    [Command(requiresAuthority = false)]
    public void CmdRequestEnter(NetworkConnectionToClient sender = null)
    {
        // 서버에서 검증: 이미 누가 타고 있으면 거절
        if (isOccupied)
        {
            TargetDenyEntry(sender);
            return;
        }

        // 소유권 부여
        isOccupied = true;
        netIdentity.AssignClientAuthority(sender);
    }

    // 클라이언트: 하차 요청
    [Command]
    public void CmdExit()
    {
        isOccupied = false;
        netIdentity.RemoveClientAuthority();
    }

    [TargetRpc]
    void TargetDenyEntry(NetworkConnectionToClient target)
    {
        Debug.Log("차량이 이미 사용 중입니다");
    }

    // 소유권 획득 시 - 조작 활성화
    public override void OnStartAuthority()
    {
        GetComponent<VehicleInput>().enabled = true;
        VehicleCamera.Instance.SetTarget(transform);
    }

    // 소유권 상실 시 - 조작 비활성화
    public override void OnStopAuthority()
    {
        GetComponent<VehicleInput>().enabled = false;
        VehicleCamera.Instance.ClearTarget();
    }
}
```

### API 레퍼런스

```csharp
// 소유권 부여 (서버에서만 호출)
[Server]
void GiveOwnership(NetworkConnectionToClient newOwner)
{
    netIdentity.AssignClientAuthority(newOwner);
}

// 소유권 제거 (서버에서만 호출)
[Server]
void RevokeOwnership()
{
    netIdentity.RemoveClientAuthority();
}

// 현재 소유자 확인
NetworkConnectionToClient owner = netIdentity.connectionToClient;
bool hasOwner = owner != null;
```

## 제한사항

### 하나의 NetworkIdentity만 허용

```
❌ 잘못된 구조:
Parent (NetworkIdentity)
└── Child (NetworkIdentity)  // 오류 발생!

✅ 올바른 구조:
Parent (NetworkIdentity)
└── Child (NetworkBehaviour만)
```

:::note
1. NetworkIdentity는 루트 오브젝트에만 추가해야 합니다
2. 프리팹에 NetworkIdentity를 추가하면 자동으로 Asset ID가 할당됩니다
3. 씬 오브젝트는 씬 저장 시 Scene ID가 자동 할당됩니다
4. NetworkIdentity가 없는 오브젝트에 NetworkBehaviour를 추가하면 에러가 발생합니다
:::
