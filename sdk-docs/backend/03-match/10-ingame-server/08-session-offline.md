---
sidebar_label: 게임방 연결 끊김 이벤트
---

# OnSessionOffline

public MatchInGameOfflineSessionEventHandler **OnSessionOffline**;

## 전달인자

| Value | Type                        | Description         |
| :---- | :-------------------------- | :------------------ |
| args  | MatchInGameSessionEventArgs | 연결 끊긴 세션 정보 |

## MatchInGameSessionEventArgs

| Value      | Type                                            | Description                            |
| :--------- | :---------------------------------------------- | :------------------------------------- |
| ErrInfo    | ErrorCode                                       | 에러 정보                              |
| Reason     | string                                          | 에러 정보                              |
| GameRecord | [MatchUserGameRecord](/sdk-docs/backend/match/argument) | 세션의 정보(세션 ID, 닉네임, 전적 등) |

## 설명

유저가 클라이언트 오류, 통신 상태 불량 등의 비정상적인 상황에서 인게임 서버와 접속이 끊어진 경우 호출되는 이벤트입니다.  

- 연결이 끊어진 클라이언트를 포함하여 게임방에 존재하는 모든 클라이언트에게 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnSessionOffline = (MatchInGameSessionEventArgs args) => {
    // TODO
};
```

## ArgumentCase

**어떤 유저의 접속이 끊어졌을 때**  
ErrInfo : ErrorCode.NetworkOffline  
Reason : "Session Disconnect"  
GameRecord : "접속이 끊어진 유저 정보"

**서버에서 연결을 끊었을 때**  
ErrInfo : ErrorCode.Exception  
Reason : "Disconnected from server."  
GameRecord : "접속이 끊어진 유저 정보"
