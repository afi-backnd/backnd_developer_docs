---
sidebar_label: 게임방 접속 요청
---

# JoinGameRoom

public void **JoinGameRoom**(string **roomToken**);

## 파라미터

| Value     | Type   | Description                                                 |
| :-------- | :----- | :---------------------------------------------------------- |
| roomToken | string | 입장할 룸 토큰(OnMatchMakingResponse에서 전달받은 룸 토큰) |

> 게임방 접속을 위한 토큰 값은 게임 [인게임 서버 접속을 위한 정보 문서](/sdk-docs/backend/match/server/request-match/join-game-server)를 참고해 주세요.  

## 설명

게임방에 접속합니다.  

- 반드시 인게임 서버 접속 후 호출되어야 합니다.  
- 매칭된 유저들이 1분 안에 모두 게임방에 접속하지 않으면 [해당 게임방은 폐기](/sdk-docs/backend/match/ingame-server/delete-session)됩니다.  
  이 경우 이미 게임방에 접속해 있던 유저들은 인게임 서버와 연결이 끊어집니다.  
- **매칭 서버의 대기방과 인게임 서버의 게임방은 분리되어 있는 별개의 개념입니다.**  

## Example

```js
Backend.Match.JoinGameRoom(roomToken);
```
