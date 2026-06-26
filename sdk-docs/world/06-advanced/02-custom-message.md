---
sidebar_label: 커스텀 메시지
---

# 커스텀 메시지

SyncVar, Command, RPC 외에 직접 네트워크 메시지를 정의할 수 있습니다. 플레이어 스폰 전 캐릭터 생성, 로비 통신 등에 활용합니다.

## 예시: 캐릭터 생성

### 1. 메시지 정의

```csharp
public struct CreateCharacterMessage : NetworkMessage
{
    public string characterName;
    public int classId;
}
```

### 2. 서버 - NetworkManager에서 처리

```csharp
public class GameNetworkManager : NetworkManager
{
    [Header("직업별 프리팹")]
    public GameObject warriorPrefab;
    public GameObject magePrefab;
    public GameObject archerPrefab;

    // 클라이언트별 캐릭터 정보 저장
    private Dictionary<NetworkConnection, CreateCharacterMessage> pendingCharacters = new Dictionary<NetworkConnection, CreateCharacterMessage>();

    public override void OnStartServer()
    {
        base.OnStartServer();
        NetworkServer.RegisterHandler<CreateCharacterMessage>(OnCreateCharacter);
    }

    void OnCreateCharacter(NetworkConnectionToClient conn, CreateCharacterMessage msg)
    {
        // 캐릭터 정보 저장 후 플레이어 스폰 요청
        pendingCharacters[conn] = msg;
        NetworkServer.AddPlayerForConnection(conn, null);
    }

    public override void OnServerAddPlayer(NetworkConnectionToClient conn)
    {
        if (!pendingCharacters.TryGetValue(conn, out var charData))
        {
            base.OnServerAddPlayer(conn);
            return;
        }

        // 직업별 프리팹 선택
        GameObject prefab = charData.classId switch
        {
            1 => warriorPrefab,
            2 => magePrefab,
            3 => archerPrefab,
            _ => playerPrefab
        };

        Transform startPos = GetStartPosition();
        GameObject player = Instantiate(prefab, startPos.position, startPos.rotation);
        player.GetComponent<Player>().playerName = charData.characterName;

        NetworkServer.AddPlayerForConnection(conn, player);
        pendingCharacters.Remove(conn);
    }
}
```

### 3. 클라이언트 - 캐릭터 생성 UI

```csharp
public class CharacterCreateUI : MonoBehaviour
{
    public void OnCreateButtonClick(string name, int classId)
    {
        var msg = new CreateCharacterMessage
        {
            characterName = name,
            classId = classId
        };
        NetworkClient.Send(msg);
    }
}
```

## 서버 → 클라이언트 전송

```csharp
// 메시지 정의
public struct GameResultMessage : NetworkMessage
{
    public bool isWin;
    public int score;
}

// 클라이언트: 핸들러 등록
void Start()
{
    NetworkClient.RegisterHandler<GameResultMessage>(OnGameResult);
}

void OnGameResult(GameResultMessage msg)
{
    Debug.Log($"결과: {(msg.isWin ? "승리" : "패배")}, 점수: {msg.score}");
}

// 서버: 특정 클라이언트에게 전송
[Server]
void SendResult(NetworkConnectionToClient conn, bool win, int score)
{
    conn.Send(new GameResultMessage { isWin = win, score = score });
}
```

## 지원 타입

네트워크 메시지 필드에 사용할 수 있는 타입은 다음과 같습니다.

| 분류         | 타입 목록                                                                 |
| ------------ | ------------------------------------------------------------------------- |
| **기본 타입**    | byte, sbyte, short, ushort, int, uint, long, ulong, float, double, decimal, bool, char, string |
| **Unity 타입**   | Vector2, Vector3, Vector4, Quaternion, Color, Color32, Rect, Plane, Ray  |
| **네트워크 타입**| NetworkIdentity, GameObject, NetworkBehaviour                            |
