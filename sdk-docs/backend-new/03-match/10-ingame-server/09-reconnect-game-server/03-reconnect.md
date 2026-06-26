---
sidebar_label: 재접속 요청
---

# JoinGameServer

public bool **JoinGameServer**(string **serverAddress**, string **serverPort**, bool **isReconnect**, out ErrorInfo **errorInfo**);

## 파라미터

| Value         | Type      | Description                                              |
| :------------ | :-------- | :------------------------------------------------------- |
| serverAddress | string    | 인게임 서버 주소(IsGameRoomActivate 전달받은 서버 주소) |
| serverPort    | ushort    | 인게임 서버 포트(IsGameRoomActivate 전달받은 서버 포트) |
| isReconnect   | bool      | 재접속 여부(true)                                       |
| errorInfo     | ErrorInfo | 성공/실패 정보                                           |

## 설명

유저가 게임 중이던 게임방에 재접속을 시도합니다.  

- 세 번째 isReconnect를 true로 주어야 정상적으로 재접속할 수 있습니다.  
- 해당 함수가 true로 리턴되면 인게임 서버와 소켓 연결에 성공한 것입니다.  
- 실제 서버 연결 및 게임방 재접속은 인게임 서버에서 유저 인증을 완료한 후 이루어지며,
  재접속 성공/실패 여부는 아래 **인게임 서버 재접속 성공/실패 유무 및 유저 인증 완료 이벤트**를 참고해 주세요.  

인게임 서버 접속과 동시에 게임방에 재접속 됩니다.  

> JoinGameRoom 함수를 호출할 필요가 없고, JoinGameRoom 함수 호출 시 InvalidOperation 에러가 발생합니다.  

### 재접속 성공 시 리턴되는 이벤트

**재접속 한 유저**  

- [인게임 서버 재접속 성공/실패 유무 및 유저 인증 완료 이벤트](/sdk-docs/backend/match/ingame-server/join-game-server/join-server-event)
- [게임방 내 유저 리스트 이벤트](/sdk-docs/backend/match/ingame-server/join-session/session-list)

**게임방에 이미 들어와 있던 유저**  

- [유저 재접속 이벤트](/sdk-docs/backend/match/ingame-server/reconnect-game-server/reconnect-success)

## Example

```js
var bro = BackndBase.Match.IsGameRoomActivate();
var roomInfo = bro.GetReturnValuetoJSON();
var addr = roomInfo["serverPublicHostName"].ToString();
var port = Convert.ToUInt16(roomInfo["serverPort"].ToString());
ErrorInfo errorInfo = null;

if(JoinGameServer(addr, port, true, out errorInfo) == false)
{
   // 에러 확인
   return;
}
```

## ReturnCase

ReturnCase는 [인게임 서버 접속 요청](/sdk-docs/backend/match/ingame-server/join-game-server/join-server)를 참고해 주세요.  
