---
sidebar_label: 파티 시스템
description: "파티 시스템"
---

# 파티 시스템

플레이어 간 파티를 구성하여 함께 플레이할 수 있는 그룹 시스템입니다.

## 기능

- 파티 생성 (공개/비공개)
- 공개 파티 참가
- 파티원 초대 및 응답
- 파티 탈퇴 / 강퇴
- 공개 파티 목록 조회

## 이벤트 등록

```csharp
void Start()
{
    NetworkClient.Party.OnUpdated += OnPartyUpdated;
    NetworkClient.Party.OnDisbanded += OnPartyDisbanded;
    NetworkClient.Party.OnInviteReceived += OnInviteReceived;
    NetworkClient.Party.OnInviteExpired += OnInviteExpired;
    NetworkClient.Party.OnListReceived += OnPartyListReceived;
    NetworkClient.Party.OnError += OnPartyError;
}

void OnDestroy()
{
    NetworkClient.Party.OnUpdated -= OnPartyUpdated;
    NetworkClient.Party.OnDisbanded -= OnPartyDisbanded;
    NetworkClient.Party.OnInviteReceived -= OnInviteReceived;
    NetworkClient.Party.OnInviteExpired -= OnInviteExpired;
    NetworkClient.Party.OnListReceived -= OnPartyListReceived;
    NetworkClient.Party.OnError -= OnPartyError;
}

void OnPartyUpdated(NetworkPartyInstance party)
{
    // 파티 정보 갱신 (생성, 참가, 탈퇴 등 모든 변경 시 호출)
    Debug.Log($"파티: {party.partyName}, 멤버: {party.memberNetIds.Count}/{party.maxMembers}");
}

void OnPartyDisbanded()
{
    // 파티에서 강퇴되었거나 파티가 해산됨
}

void OnInviteReceived(NetworkPartyInviteReceivedMessage invite)
{
    // 다른 플레이어로부터 파티 초대를 받음
    Debug.Log($"파티 초대: {invite.partyName}");
}

void OnInviteExpired(ulong partyId)
{
    // 파티 초대가 만료됨 (5분 경과)
    Debug.Log($"초대 만료: {partyId}");
}

void OnPartyListReceived(IReadOnlyList<NetworkPartyListItem> parties)
{
    // 공개 파티 목록 수신
    foreach (var party in parties)
    {
        Debug.Log($"{party.partyName} ({party.memberCount}/{party.maxMembers})");
    }
}

void OnPartyError(string error)
{
    // 오류 발생
    Debug.LogWarning(error);
}
```

:::warning 이벤트 해제
`OnDestroy()`에서 반드시 이벤트를 해제하세요. 해제하지 않으면 메모리 누수가 발생할 수 있습니다.
:::

## 파티 생성

```csharp
NetworkClient.Party.Create("우리 파티", maxMembers: 4, isPublic: false);
```

- `name`: 파티 이름
- `maxMembers`: 최대 인원 (기본값: 4, 최소: 2)
- `isPublic`: 공개 파티 여부 (기본값: false)
  - `true`: 누구나 참가 가능, 목록에 노출
  - `false`: 초대를 통해서만 참가 가능

생성이 완료되면 `OnUpdated` 이벤트가 호출됩니다.

## 공개 파티 참가

```csharp
NetworkClient.Party.Join(partyId);
```

`isPublic = true`인 파티에만 직접 참가할 수 있습니다.

## 파티 탈퇴

```csharp
NetworkClient.Party.Leave();
```

- 파티장이 탈퇴하면 다음 멤버가 자동으로 파티장이 됩니다
- 마지막 멤버가 탈퇴하면 파티가 해산됩니다

:::tip 파티 해산
파티장이 명시적으로 파티를 해산하는 API는 제공되지 않습니다. 파티를 해산하려면 모든 멤버가 탈퇴해야 합니다.
:::

## 파티원 초대

파티장만 초대할 수 있습니다.

```csharp
NetworkClient.Party.Invite(targetPlayerNetId);
```

대상 플레이어의 `OnInviteReceived` 이벤트가 호출됩니다.

## 초대 응답

```csharp
// 수락
NetworkClient.Party.AcceptInvite(partyId);

// 거절
NetworkClient.Party.DeclineInvite(partyId);
```

:::info 초대 자동 만료
파티 초대는 **5분**이 지나면 자동으로 만료됩니다. 만료 시 `OnInviteExpired` 이벤트가 호출되며, `PendingInvites` 목록에서 자동으로 제거됩니다.
:::

## 파티원 강퇴

파티장만 강퇴할 수 있습니다.

```csharp
NetworkClient.Party.Kick(targetPlayerNetId);
```

강퇴된 플레이어의 `OnDisbanded` 이벤트가 호출됩니다.

## 공개 파티 목록 조회

```csharp
NetworkClient.Party.RequestList();
```

참가 가능한 공개 파티 목록이 `OnListReceived` 이벤트로 전달됩니다. 정원이 찬 파티는 목록에서 제외됩니다.

## 상태 조회

```csharp
// 파티 소속 여부
if (NetworkClient.Party.IsInParty)
{
    Debug.Log($"파티명: {NetworkClient.Party.PartyName}");
    Debug.Log($"멤버 수: {NetworkClient.Party.MemberCount}/{NetworkClient.Party.MaxMembers}");
}

// 파티장 여부
if (NetworkClient.Party.IsLeader)
{
    // 초대, 강퇴 가능
}

// 파티원 목록
foreach (ulong netId in NetworkClient.Party.MemberNetIds)
{
    Debug.Log($"멤버 NetId: {netId}");
}

// 받은 초대 목록
IReadOnlyCollection<ulong> invites = NetworkClient.Party.PendingInvites;
```

### 파티 프로퍼티

| 프로퍼티 | 타입 | 설명 |
|----------|------|------|
| `PartyId` | `ulong` | 파티 고유 ID |
| `PartyName` | `string` | 파티 이름 |
| `LeaderNetId` | `ulong` | 파티장의 NetId |
| `MemberNetIds` | `IReadOnlyList<ulong>` | 파티원 NetId 목록 (읽기 전용) |
| `MemberCount` | `int` | 현재 파티원 수 |
| `MaxMembers` | `int` | 최대 인원 |
| `IsPublic` | `bool` | 공개 파티 여부 |
| `IsInParty` | `bool` | 파티 소속 여부 |
| `IsLeader` | `bool` | 파티장 여부 |

## 파티와 인스턴스 던전

파티원과 함께 인스턴스 던전에 입장할 수 있습니다.

```csharp
// 파티원 모두 같은 인스턴스에 입장
NetworkClient.Party.LoadNetworkScene("Dungeon01");
```

:::note 던전 퇴장 후 파티 정리
인스턴스 던전 입장 후 파티 정보를 유지할지, 정리할지는 게임 정책마다 다를 수 있어 자동으로 파티를 정리하지 않습니다. 게임 정책에 맞게 파티 탈퇴 코드를 작성하세요.

아래는 던전 퇴장 후 파티를 탈퇴하는 예시입니다.

```csharp
// 던전 퇴장 후 파티 탈퇴
NetworkClient.UnloadNetworkScene();

if (NetworkClient.Party.IsInParty)
{
    NetworkClient.Party.Leave();
}
```
:::

자세한 내용은 [NetworkScene](./01-network-scene.md)을 참고하세요.
