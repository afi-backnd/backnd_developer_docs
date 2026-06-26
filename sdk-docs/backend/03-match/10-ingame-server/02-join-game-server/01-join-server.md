---
sidebar_label: 인게임 서버 접속 요청
---

# JoinGameServer

public bool **JoinGameServer**(string **serverAddress**, string **serverPort**, bool **isReconnect**, out ErrorInfo **errorInfo**);

## 파라미터

| Value         | Type      | Description                                                                                                                                   |
| :------------ | :-------- | :-------------------------------------------------------------------------------------------------------------------------------------------- |
| serverAddress | string    | 인게임 서버 주소(OnMatchMakingResponse의 roomInfo에서 전달받은 서버 주소)                                                                    |
| serverPort    | ushort    | 인게임 서버 포트(OnMatchMakingResponse의 roomInfo에서 전달받은 서버 포트)                                                                    |
| isReconnect   | bool      | 재접속 여부(false). 재접속을 수행하는 법은 [재접속 문서](/sdk-docs/backend/match/ingame-server/reconnect-game-server/reconnect)를 참고해 주세요. |
| errorInfo     | ErrorInfo | 성공/실패 정보                                                                                                                                |

> 인게임 서버 접속을 위한 서버 주소, 포트에 대한 정보는 [인게임 서버 접속을 위한 정보 문서](/sdk-docs/backend/match/server/request-match/join-game-server)를 확인해 주세요.  

## 설명

인게임 서버에 접속을 시도합니다.  

- 인게임 서버 접속 함수의 리턴이 true인 것은 서버 접속에 성공한 것이 아닌 단순히 서버와 소켓이 연결된 것입니다.  
- 실제 서버 연결은 인게임 서버에서 유저 인증을 완료한 후 이루어지며, **인게임 서버의 접속 성공/실패 여부는 [OnSessionJoinInServer](/sdk-docs/backend/match/ingame-server/join-game-server/join-server-event) 이벤트를 통해 확인**할 수 있습니다.  

## Example

```js
string serverAddress;
ushort serverPort;

OnMatchMakingResponse += (args) => // serverAddress와 serverPort 확인하는 곳
{
    string serverAddress = args.RoomInfo.m_inGameServerEndPoint.m_address;
    ushort serverPort = args.RoomInfo.m_inGameServerEndPoint.m_port;
}

void JoinInGameServer() // 임의의 함수
{
    bool isReconnect = true;
    ErrorInfo errorInfo = null;

    if(Backend.Match.JoinGameServer(serverAddress, serverPort, isReconnect, out errorInfo) == false)
    {
        // 에러 확인
        return;
    }
}

```

## ReturnCase

**Success cases**  
인게임 서버와 소켓 연결에 성공한 경우  
true  
errorInfo : ErrorInfo.Success

**Error cases**  
**인게임 서버 접속에 실패한 경우**  
false  
errorInfo.Category : ErrorCode.Exception  
errorInfo.Reasion : "이유"  

**인게임 서버와 소켓 연결에 실패한 경우**  
false  
errorInfo.Category : ErrorCode.SocketOperationError  
errorInfo.SocketError : 발생한 소켓 에러 타입  
errorInfo.Reason : "이유"
