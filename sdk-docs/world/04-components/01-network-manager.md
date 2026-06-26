---
sidebar_label: NetworkManager
description: "NetworkManager"
---

# NetworkManager

월드의 핵심 컴포넌트입니다. 씬에 하나만 배치하면 서버 연결, 플레이어 스폰, 씬 전환 등 멀티플레이어 게임의 기반 시스템이 모두 준비됩니다.

복잡한 네트워크 인프라 구축 없이 게임 로직 개발에 집중할 수 있습니다.

NetworkManager의 주요 기능은 다음과 같습니다:
- 게임 상태 관리
- 스폰 관리
- 씬 관리

:::note
각 씬에는 하나의 활성화된 NetworkManager만 존재할 수 있습니다(싱글톤). NetworkIdentity 컴포넌트가 있는 네트워크 게임 오브젝트에는 NetworkManager 컴포넌트를 배치하지 마세요. 씬 로드 시 이러한 오브젝트가 비활성화되기 때문입니다.
:::

## 게임 상태 관리하기

네트워크 멀티플레이어 게임은 다음과 같은 세 가지 모드로 실행될 수 있습니다:

| 모드 | 설명 |
|------|------|
| 클라이언트 | 서버에 접속하는 일반 플레이어 모드 |
| 전용 서버 | 게임을 호스팅하는 서버 전용 모드 |
| 호스트 | 동일 프로세스에서 서버와 클라이언트를 동시에 실행하는 모드 **(개발/테스트용)** |

### 호스트 모드 (개발용)

호스트 모드는 **로컬 개발 및 테스트 전용**입니다. 네트워크 통신 없이 동일 프로세스 내에서 서버와 클라이언트가 직접 통신하므로 빠른 테스트가 가능합니다.

:::warning 주의
호스트 모드는 프로덕션 환경에서 사용할 수 없습니다. 실제 서비스에서는 서버와 클라이언트를 분리하여 빌드/배포해야 합니다.
:::

**호스트 모드 특징:**
- 네트워크 지연 없이 즉시 테스트 가능
- 별도의 서버 실행 없이 개발 가능
- 디버깅이 용이함 (서버/클라이언트 코드가 동일 프로세스에서 실행)

**프로덕션 환경:**
1. 서버는 클라우드에 배포 ([빌드 & 배포](../../build-deploy) 참고)
2. 클라이언트는 `StartClient()`로 서버에 연결

### 시작 메서드

자체 UI를 구현할 때는 다음 메서드들을 사용하여 각 모드를 시작할 수 있습니다:
- StartClient: 클라이언트 모드로 시작
- StartServer: 전용 서버 모드로 시작
- StartHost: 호스트 모드로 시작 (개발/테스트용)

### 서버 상태 확인하기

서버에 연결하기 전에 서버의 현재 상태와 접속 인원을 확인할 수 있습니다. 이를 통해 서버가 정상 운영 중인지, 현재 접속자 수가 얼마인지 미리 파악하여 유저에게 적절한 안내를 제공할 수 있습니다.

#### ServerState

서버 상태를 나타내는 열거형입니다:

| 상태 | 설명 |
|------|------|
| `NotChecked` | 아직 상태를 확인하지 않음 |
| `Running` | 서버가 정상 운영 중 |
| `Stopped` | 서버가 중지됨 |
| `Error` | 서버 오류 발생 |

#### LoadServerStatus

서버 상태를 조회합니다. 조회가 완료되면 `OnServerStatus` 콜백이 호출됩니다.

```csharp
// 서버 상태 조회 요청
NetworkManager.Instance.LoadServerStatus();
```

#### OnServerStatus

서버 상태 조회가 완료되었을 때 호출되는 콜백입니다. NetworkManager를 상속하여 오버라이드할 수 있습니다.

```csharp
public class MyNetworkManager : NetworkManager
{
    public override void OnServerStatus(ServerState state, int currentConnections, int maxConnections)
    {
        switch (state)
        {
            case ServerState.Running:
                Debug.Log($"서버 정상 운영 중 - 접속자: {currentConnections}/{maxConnections}");

                if (currentConnections >= maxConnections)
                {
                    // 서버가 가득 찬 경우 안내
                    UIManager.Instance.ShowServerFullMessage();
                }
                else
                {
                    // 접속 가능 - 클라이언트 시작
                    StartClient();
                }
                break;

            case ServerState.Stopped:
                Debug.Log("서버가 중지되었습니다");
                UIManager.Instance.ShowServerMaintenanceMessage();
                break;

            case ServerState.Error:
                Debug.LogError("서버 상태 확인 중 오류 발생");
                UIManager.Instance.ShowServerErrorMessage();
                break;

            case ServerState.NotChecked:
                Debug.Log("서버 상태 확인 중...");
                break;
        }
    }
}
```

:::tip 활용 팁
- 게임 시작 화면에서 서버 상태를 표시하여 유저에게 접속 가능 여부를 안내할 수 있습니다.
- 접속 인원이 최대치에 가까우면 대기열 시스템을 구현하거나 다른 서버로 안내할 수 있습니다.
- 서버 점검 중일 때 적절한 메시지를 표시하여 유저 경험을 개선할 수 있습니다.
:::

### 월드 서비스 연결하기

:::note
월드 서비스에 연결하기 위해서는 NetworkManager의 `Use Cloud` 변수를 반드시 `true`로 설정해야 합니다.
:::

## 스폰 관리하기

NetworkManager를 사용하여 프리팹에서 네트워크 게임 오브젝트의 스폰(네트워크 인스턴스화)을 관리할 수 있습니다.

대부분의 게임에는 플레이어를 나타내는 프리팹이 있으므로, NetworkManager에는 Player Prefab 슬롯이 있습니다. 이 슬롯에 플레이어 프리팹을 할당해야 합니다. 플레이어 프리팹이 설정되면 게임의 각 사용자에 대해 해당 프리팹에서 플레이어 게임 오브젝트가 자동으로 생성됩니다.

:::note
Player Prefab을 이 필드에 할당하기 전에 NetworkIdentity 컴포넌트를 추가해야 합니다.
:::

**개발 환경에서 테스트하기 (호스트 모드):**
1. 호스트 모드로 게임을 시작하면 플레이어 게임 오브젝트가 생성됩니다.
2. 게임을 중지하면 플레이어 게임 오브젝트가 제거됩니다.
3. 다른 게임 클라이언트를 연결하면 새로운 플레이어 게임 오브젝트가 생성됩니다.
4. 클라이언트를 중지하면 해당 플레이어의 게임 오브젝트가 제거됩니다.

### 시작 위치 설정하기

NetworkManager는 기본적으로 정의된 transform 위치와 회전값에서 Player Prefab을 생성합니다. Player Spawn Method 속성을 사용하여 NetworkStartPosition 컴포넌트와 함께 시작 위치 선택 방법을 제어할 수 있습니다. 자세한 내용은 [NetworkStartPosition](../network-start-position) 문서를 참고하세요.

### 스폰 가능한 프리팹 등록하기

플레이어 외에 런타임에 스폰되는 네트워크 오브젝트(몬스터, 아이템, 투사체 등)는 NetworkManager의 **Registered Spawnable Prefabs** 목록에 등록해야 합니다. 클라이언트가 서버로부터 스폰 메시지를 받았을 때, 해당 프리팹이 등록되어 있어야 오브젝트를 생성할 수 있습니다. 등록되지 않은 프리팹을 스폰하면 클라이언트에서 오류가 발생합니다.

**등록 방법:**
1. NetworkManager 인스펙터에서 Registered Spawnable Prefabs 목록에 프리팹 추가
2. 또는 코드에서 `NetworkClient.RegisterPrefab(prefab)` 호출

```csharp
// 서버에서 몬스터 스폰
GameObject monster = Instantiate(monsterPrefab, spawnPosition, Quaternion.identity);
NetworkServer.Spawn(monster);
```

:::note
- 모든 스폰 프리팹에는 NetworkIdentity 컴포넌트가 필요합니다.
- Player Prefab은 Registered Spawnable Prefabs 목록에 별도로 추가할 필요 없습니다.
- 캐릭터 직업별 프리팹은 Player Prefab 대신 Registered Spawnable Prefabs에 하신 후 커스텀 메시지를 통해 스폰하는 방식을 권장합니다 (자세한 내용은 [커스텀 메시지](../../advanced/custom-message) 문서 참고).
:::

## 씬 관리하기

대부분의 게임은 여러 씬으로 구성됩니다. NetworkManager는 멀티플레이어 게임에 적합한 방식으로 씬 상태와 씬 전환을 자동으로 관리하도록 설계되었습니다.

NetworkManager 인스펙터에는 두 개의 씬 슬롯이 있습니다:
- Offline Scene
- Online Scene

이 슬롯에 씬 에셋을 할당하면 네트워크 씬 관리가 활성화됩니다.

씬 관리 동작:
1. 서버가 시작되면 Online Scene이 로드됩니다 (호스트 모드에서도 동일).
2. 이 씬이 현재 네트워크 씬이 됩니다.
3. 서버에 연결하는 모든 클라이언트는 이 씬을 로드하도록 지시받습니다.
4. 이 씬의 이름은 networkSceneName 속성에 저장됩니다.
5. 네트워크가 중지되면(서버 중지 또는 클라이언트 연결 해제) Offline Scene이 로드됩니다.

게임이 활성화된 상태에서 ServerChangeScene을 호출하여 씬을 변경할 수 있습니다:
- 현재 연결된 모든 클라이언트의 씬이 변경됩니다.
- networkSceneName이 업데이트되어 새 클라이언트도 새 씬을 로드합니다.

:::note
1. 씬 변경 시 이전 씬의 모든 게임 오브젝트가 제거됩니다.
2. NetworkManager는 씬 간에 유지되어야 합니다. 그렇지 않으면 씬 변경 시 네트워크 연결이 끊어집니다.
3. 이를 위해 인스펙터에서 Don't Destroy On Load 체크박스를 활성화하세요.
:::

### NetworkScene

파티별 던전, 개인 공간 등 독립적인 인스턴스 씬이 필요한 경우 **NetworkScene**을 사용합니다.

NetworkManager Inspector의 **Network Scenes** 배열에 씬을 등록하면:
- `NetworkClient.LoadNetworkScene()`으로 인스턴스 생성 및 입장
- 파티원과 함께 입장 가능
- 빈 인스턴스는 자동 정리

자세한 내용은 [NetworkScene](../../advanced/network-scene) 문서를 참고하세요.

## 가상 함수 (Virtual Methods)

NetworkManager를 상속하여 커스텀 NetworkManager를 만들 때 다음 가상 함수들을 오버라이드할 수 있습니다.

### 가상 함수 목록

| 함수명 | 호출 시점 |
|--------|----------|
| `OnStartHost` | 호스트 시작 시 (개발/테스트용) |
| `OnStopHost` | 호스트 중지 시 (개발/테스트용) |
| `OnStartServer` | 서버 시작 시 |
| `OnStopServer` | 서버 중지 시 |
| `OnStartClient` | 클라이언트 시작 시 |
| `OnStopClient` | 클라이언트 중지 시 |
| `OnServerConnect` | 클라이언트가 서버에 연결 시 |
| `OnServerDisconnect` | 클라이언트가 서버에서 연결 해제 시 |
| `OnServerReady` | 클라이언트가 준비 완료 시 |
| `OnServerAddPlayer` | 플레이어 추가 요청 시 |
| `OnServerError` | 서버 에러 발생 시 |
| `OnServerChangeScene` | 서버 씬 변경 시작 시 |
| `OnServerSceneChanged` | 서버 씬 변경 완료 시 |
| `OnServerStatus` | 서버 상태 조회 완료 시 |
| `OnClientConnect` | 서버에 연결 성공 시 |
| `OnClientDisconnect` | 서버에서 연결 해제 시 |
| `OnClientError` | 클라이언트 에러 발생 시 |
| `OnClientNotReady` | 클라이언트 준비 해제 시 |
| `OnClientChangeScene` | 클라이언트 씬 변경 시작 시 |
| `OnClientSceneChanged` | 클라이언트 씬 변경 완료 시 |
| `GetStartPosition` | 플레이어 시작 위치 결정 시 |
| `ServerChangeScene` | 서버에서 씬 변경 요청 시 |

### 서버 이벤트 콜백

#### OnStartServer / OnStopServer

```csharp
public class MyNetworkManager : NetworkManager
{
    public override void OnStartServer()
    {
        base.OnStartServer();
        Debug.Log("서버가 시작되었습니다");
        // 서버 초기화 로직
    }

    public override void OnStopServer()
    {
        base.OnStopServer();
        Debug.Log("서버가 중지되었습니다");
        // 서버 정리 로직
    }
}
```

#### OnServerConnect / OnServerDisconnect

```csharp
public override void OnServerConnect(NetworkConnectionToClient conn)
{
    base.OnServerConnect(conn);
    Debug.Log($"클라이언트 연결: {conn.connectionId}");
}

public override void OnServerDisconnect(NetworkConnectionToClient conn)
{
    Debug.Log($"클라이언트 연결 해제: {conn.connectionId}");
    base.OnServerDisconnect(conn); // 플레이어 오브젝트 제거
}
```

#### OnServerReady

클라이언트가 준비 완료 상태가 되었을 때 호출됩니다.

```csharp
public override void OnServerReady(NetworkConnectionToClient conn)
{
    base.OnServerReady(conn); // NetworkServer.SetClientReady(conn) 호출
    Debug.Log($"클라이언트 준비 완료: {conn.connectionId}");

    // 준비된 클라이언트에게 게임 상태 전송 등
}
```

#### OnServerAddPlayer

플레이어 스폰 로직을 커스터마이징할 때 사용합니다.

```csharp
public override void OnServerAddPlayer(NetworkConnectionToClient conn)
{
    // 커스텀 시작 위치 결정
    Transform startPos = GetCustomStartPosition(conn);

    // 플레이어 생성
    GameObject player = Instantiate(playerPrefab, startPos.position, startPos.rotation);

    // 추가 초기화
    player.GetComponent<Player>().Initialize(conn);

    // 네트워크에 스폰
    NetworkServer.AddPlayerForConnection(conn, player);
}
```

#### 직업별 캐릭터 스폰

클라이언트에서 직업을 선택한 후 서버에서 해당 프리팹을 스폰하는 예시는 [커스텀 메시지](../../advanced/custom-message) 문서를 참고하세요.

#### OnServerChangeScene / OnServerSceneChanged

```csharp
public override void OnServerChangeScene(string newSceneName)
{
    base.OnServerChangeScene(newSceneName);
    Debug.Log($"씬 변경 시작: {newSceneName}");
}

public override void OnServerSceneChanged(string sceneName)
{
    base.OnServerSceneChanged(sceneName);
    // 월드 씬 로드 완료 시 필드 몬스터/NPC 스폰
    WorldManager.Instance.SpawnFieldEntities();
}
```

#### OnServerError

```csharp
public override void OnServerError(NetworkConnectionToClient conn, TransportError error, string reason)
{
    base.OnServerError(conn, error, reason);
    Debug.LogError($"서버 에러 [{conn.connectionId}]: {error} - {reason}");
}
```

#### OnServerStatus

서버 상태 조회가 완료되었을 때 호출됩니다. 자세한 내용은 [서버 상태 확인하기](#서버-상태-확인하기) 섹션을 참고하세요.

### 클라이언트 이벤트 콜백

#### OnStartClient / OnStopClient

```csharp
public override void OnStartClient()
{
    base.OnStartClient();
    Debug.Log("클라이언트가 시작되었습니다");
    // 클라이언트 초기화
}

public override void OnStopClient()
{
    base.OnStopClient();
    Debug.Log("클라이언트가 중지되었습니다");
    // 클라이언트 정리
}
```

#### OnClientConnect / OnClientDisconnect

```csharp
public override void OnClientConnect()
{
    base.OnClientConnect();
    Debug.Log("서버에 연결되었습니다");
    // 로비 UI 표시 등
}

public override void OnClientDisconnect()
{
    base.OnClientDisconnect();
    Debug.Log("서버에서 연결이 해제되었습니다");
    // 재연결 UI 표시 등
}
```

#### OnClientChangeScene / OnClientSceneChanged

```csharp
public override void OnClientChangeScene(string newSceneName, SceneOperation sceneOperation, bool customHandling)
{
    base.OnClientChangeScene(newSceneName, sceneOperation, customHandling);
    Debug.Log($"클라이언트 씬 변경 시작: {newSceneName}");
    // 로딩 화면 표시
    LoadingScreen.Instance.Show();
}

public override void OnClientSceneChanged()
{
    base.OnClientSceneChanged();
    Debug.Log("클라이언트 씬 변경 완료");
    // 로딩 화면 숨김
    LoadingScreen.Instance.Hide();
}
```

#### OnClientError

```csharp
public override void OnClientError(TransportError error, string reason)
{
    base.OnClientError(error, reason);
    Debug.LogError($"클라이언트 에러: {error} - {reason}");
    // 에러 팝업 표시
    ErrorPopup.Instance.Show(reason);
}
```

### 호스트 이벤트 콜백 (개발/테스트용)

호스트 모드 전용 콜백입니다. 개발 환경에서 테스트할 때 사용됩니다.

```csharp
public override void OnStartHost()
{
    base.OnStartHost();
    Debug.Log("호스트가 시작되었습니다 (개발 모드)");
}

public override void OnStopHost()
{
    base.OnStopHost();
    Debug.Log("호스트가 중지되었습니다");
}
```

:::tip 개발 팁
호스트 모드에서 `OnStartHost()`가 호출되면 `OnStartServer()`와 `OnStartClient()`도 순차적으로 호출됩니다. 따라서 프로덕션과 개발 환경 모두에서 동작하는 로직은 `OnStartServer()`나 `OnStartClient()`에 작성하세요.
:::

### 유틸리티 가상 함수

#### GetStartPosition

플레이어 시작 위치를 커스터마이징합니다.

```csharp
public override Transform GetStartPosition()
{
    // 신규 플레이어는 마을에서 시작
    if (IsNewPlayer())
        return townSpawnPoint;

    // 기존 플레이어는 마지막 위치 근처에서 시작
    return GetNearestSpawnPoint(lastSavedPosition);
}
```

#### ServerChangeScene

서버에서 씬을 변경합니다.

```csharp
public override void ServerChangeScene(string newSceneName)
{
    // 씬 변경 전 정리
    SaveGameState();

    base.ServerChangeScene(newSceneName);
}
```

### 커스텀 NetworkManager 예시

```csharp
public class GameNetworkManager : NetworkManager
{
    public static new GameNetworkManager Instance => (GameNetworkManager)singleton;

    // ===== 서버 콜백 =====

    public override void OnStartServer()
    {
        base.OnStartServer();
        // 월드 초기화 (NPC, 필드 몬스터 등)
        WorldManager.Instance.InitializeWorld();
    }

    public override void OnServerConnect(NetworkConnectionToClient conn)
    {
        base.OnServerConnect(conn);
        Debug.Log($"플레이어 접속: {conn.connectionId}");
    }

    public override void OnServerDisconnect(NetworkConnectionToClient conn)
    {
        // 플레이어 데이터 저장
        if (conn.identity != null)
        {
            var player = conn.identity.GetComponent<Player>();
            player?.SaveData();
        }

        base.OnServerDisconnect(conn);
    }

    public override void OnServerAddPlayer(NetworkConnectionToClient conn)
    {
        // 스폰 위치 결정
        Transform spawnPoint = GetStartPosition();

        GameObject player = Instantiate(playerPrefab, spawnPoint.position, spawnPoint.rotation);
        NetworkServer.AddPlayerForConnection(conn, player);
    }

    // ===== 클라이언트 콜백 =====

    public override void OnClientConnect()
    {
        base.OnClientConnect();
        UIManager.Instance.ShowWorldUI();
    }

    public override void OnClientDisconnect()
    {
        base.OnClientDisconnect();
        UIManager.Instance.ShowReconnectUI();
    }

    public override void OnClientSceneChanged()
    {
        base.OnClientSceneChanged();
        LoadingScreen.Instance.Hide();
    }
}
```

:::note
1. 가상 함수 오버라이드 시 `base.함수명()`을 호출하여 기본 동작을 유지하세요.
2. `OnServerDisconnect`에서 `base.OnServerDisconnect(conn)`을 호출하면 플레이어 오브젝트가 자동 제거됩니다.
3. `NetworkManager.Instance`을 통해 싱글톤 인스턴스에 접근할 수 있습니다.
:::
