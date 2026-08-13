---
sidebar_label: "다른 유저 재접속 성공 이벤트"
description: "OnSessionOnline"
---

# OnSessionOnline

public MatchInGameOnlineSessionEventHandler **OnSessionOnline**;

## 전달인자

| Value | Type                        | Description        |
| :---- | :-------------------------- | :----------------- |
| args  | MatchInGameSessionEventArgs | 재접속한 세션 정보 |

## MatchInGameSessionEventArgs

| Value      | Type                                            | Description                            |
| :--------- | :---------------------------------------------- | :------------------------------------- |
| ErrInfo    | ErrorCode                                       | 에러 정보                              |
| Reason     | string                                          | 에러 정보                              |
| GameRecord | [MatchUserGameRecord](/sdk-docs/backend/match/argument) | 세션의 정보(세션 ID, 닉네임, 전적 등) |

## 설명

다른 유저가 게임방에 재접속 성공했을 경우 호출되는 이벤트 핸들러입니다.  
**자기 자신이 게임방에 재접속 성공했을 때는 호출되지 않습니다.**  

## Example

```js
Backend.Match.OnSessionOnline = (MatchInGameSessionEventArgs args) => {
    // TODO
};
```

## ArgumentCase

**다른 유저가 재접속 성공했을 경우**  
ErrInfo : ErrorCode.NetworkOnline  
Reason : "Session Reconnect"  
GameRecord : "세션의 정보"
