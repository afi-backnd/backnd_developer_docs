---
sidebar_label: NetworkScene
description: "NetworkScene"
---

# NetworkScene

파티별 전용 던전, 개인 공간 등 독립적인 NetworkScene을 생성하고 관리합니다.

`NetworkClient.LoadNetworkScene()` 한 줄로 NetworkScene 생성, 플레이어 이동, 빈 NetworkScene 정리까지 자동으로 처리됩니다.

## 사용 사례

- **인스턴스 던전**: 파티별 전용 던전
- **개인 공간**: 플레이어별 하우징/개인 영역
- **매칭 기반 게임**: PvP 아레나, 미니게임 등

## 설정

### 1. NetworkManager에 씬 등록

NetworkManager Inspector의 **Network Scenes** 배열에 NetworkScene으로 사용할 씬을 추가합니다.

```csharp
[Serializable]
public class NetworkScene
{
    public string scene;              // 씬 경로
    public int maxPlayers;            // 최대 플레이어 수
    public bool clientLoadAdditive;   // 클라이언트 Additive 로드 여부
}
```

### 2. Inspector 설정 항목

| 항목 | 설명 |
|------|------|
| **Scene** | NetworkScene으로 사용할 씬 선택 |
| **Max Players** | 해당 NetworkScene의 최대 동시 접속자 |
| **Client Load Additive** | 클라이언트의 씬 로드 방식 |

### 3. Client Load Additive 옵션

| 값 | 동작 | 사용 사례 |
|----|------|----------|
| `false` | 기존 씬 교체 (Normal) | 독립적인 던전 씬 |
| `true` | 기존 씬 유지 + 추가 로드 (Additive) | UI/매니저 씬을 유지해야 할 때 |

**Additive 구조 예시:**
```
GameScene (UI, 매니저 - 항상 유지)
├── FieldScene (Additive)
└── DungeonScene (Additive - 입장/퇴장)
```

### 4. Build Settings

네트워크 씬은 반드시 **Build Settings**에 포함되어야 합니다.

## API

### 씬 입장

```csharp
public static void NetworkClient.LoadNetworkScene(string sceneName, List<ulong> playerNetIds = null)
```

| 파라미터 | 설명 |
|---------|------|
| `sceneName` | NetworkManager의 Network Scenes에 등록된 씬 이름 |
| `playerNetIds` | 함께 입장할 플레이어들의 NetID (선택) |

- 요청자는 **자동으로 추가**되므로 `playerNetIds`에 포함하지 않아도 됩니다.
- `playerNetIds`가 null이면 요청자 혼자 입장합니다.

### 씬 퇴장

```csharp
public static void NetworkClient.UnloadNetworkScene()
```

- 현재 NetworkScene에서 퇴장합니다.
- 씬 핸들은 내부적으로 관리되므로 별도 저장이 불필요합니다.
- `clientLoadAdditive` 설정에 따라 Additive 언로드 또는 Online Scene으로 이동합니다.

## 사용 예시

### 솔로 던전 입장

```csharp
public class DungeonEntrance : MonoBehaviour
{
    public void EnterDungeon()
    {
        NetworkClient.LoadNetworkScene("Dungeon_Forest");
    }
}
```

### 파티 던전 입장

```csharp
public class DungeonEntrance : MonoBehaviour
{
    public void EnterDungeonWithParty()
    {
        if (!NetworkClient.Party.IsInParty)
        {
            Debug.Log("파티에 먼저 가입하세요");
            return;
        }

        // 파티원 모두 같은 인스턴스에 입장
        NetworkClient.Party.LoadNetworkScene("Dungeon_Raid");
    }
}
```

### 던전 퇴장

```csharp
public class DungeonExit : MonoBehaviour
{
    public void ExitDungeon()
    {
        NetworkClient.UnloadNetworkScene();
    }
}
```

## NetworkScene 내 오브젝트 스폰

NetworkScene에서 오브젝트를 스폰할 때는 `NetworkServer.SpawnInNetworkScene()`을 사용합니다.

```csharp
public static void NetworkServer.SpawnInNetworkScene(GameObject obj, GameObject root, NetworkConnection ownerConnection = null)
```

| 파라미터 | 설명 |
|---------|------|
| `obj` | 스폰할 오브젝트 |
| `root` | 씬을 참조할 기준 오브젝트 (해당 오브젝트가 속한 씬에 스폰됨) |
| `ownerConnection` | 소유자 연결 (선택) |

```csharp
public class DungeonSpawner : NetworkBehaviour
{
    public GameObject monsterPrefab;
    public Transform[] spawnPoints;

    public override void OnStartServer()
    {
        SpawnMonsters();
    }

    [Server]
    void SpawnMonsters()
    {
        foreach (var point in spawnPoints)
        {
            GameObject monster = Instantiate(monsterPrefab, point.position, point.rotation);

            // 현재 오브젝트(this.gameObject)가 속한 씬에 스폰
            NetworkServer.SpawnInNetworkScene(monster, gameObject);
        }
    }
}
```

## 플레이어 스폰 위치

NetworkScene에 `NetworkStartPosition` 컴포넌트를 배치하면 입장한 플레이어가 해당 위치에 자동 배치됩니다.

- **Random**: 여러 NetworkStartPosition 중 무작위 선택
- **RoundRobin**: 순차 선택 (현재 구현에서는 첫 번째 사용)

NetworkStartPosition이 없으면 플레이어 위치가 변경되지 않습니다.

## 에러 처리

```csharp
public class SceneErrorHandler : MonoBehaviour
{
    void Start()
    {
        NetworkClient.RegisterHandler<NetworkSceneErrorMessage>(OnSceneError);
    }

    void OnSceneError(NetworkSceneErrorMessage msg)
    {
        Debug.LogError($"씬 에러: {msg.errorMessage}");
    }
}
```

### 에러 메시지

| 에러 | 원인 |
|------|------|
| `NetworkScene '{name}' not found` | NetworkManager에 씬이 등록되지 않음 |
| `Scene '{name}' is full` | maxPlayers 초과 |
| `You are not part of this scene` | 해당 씬에 없는 플레이어가 퇴장 요청 |
| `Scene with handle {handle} not found` | 유효하지 않은 씬 핸들로 퇴장 요청 |

## 자동 정리

빈 NetworkScene은 자동으로 정리됩니다. 플레이어가 없는 NetworkScene은 일정 시간 후 서버에서 자동으로 언로드됩니다.

## 전체 예시

```csharp
public class DungeonManager : MonoBehaviour
{
    [Header("던전 설정")]
    public string dungeonSceneName = "Dungeon_Boss";
    public int requiredPlayers = 4;

    void Start()
    {
        // 에러 핸들러 등록
        NetworkClient.RegisterHandler<NetworkSceneErrorMessage>(OnSceneError);
    }

    // 던전 입장
    public void EnterDungeon()
    {
        if (!NetworkClient.Party.IsInParty)
        {
            Debug.Log("파티에 먼저 가입하세요");
            return;
        }

        if (NetworkClient.Party.MemberCount < requiredPlayers)
        {
            Debug.Log($"최소 {requiredPlayers}명 필요");
            return;
        }

        // 파티원 모두 같은 인스턴스에 입장
        NetworkClient.Party.LoadNetworkScene(dungeonSceneName);
    }

    // 던전 퇴장
    public void ExitDungeon()
    {
        NetworkClient.UnloadNetworkScene();
    }

    void OnSceneError(NetworkSceneErrorMessage msg)
    {
        Debug.LogError(msg.errorMessage);
    }
}
```

:::note
1. NetworkScene은 **Build Settings**에 포함되어야 합니다
2. 씬 내 오브젝트 스폰 시 `NetworkServer.SpawnInNetworkScene()` 사용
3. `NetworkStartPosition` 배치로 플레이어 스폰 위치 지정
4. 빈 NetworkScene은 자동 정리
:::
