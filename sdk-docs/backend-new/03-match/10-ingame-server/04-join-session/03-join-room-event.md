---
sidebar_label: 게임방 접속 이벤트
---

# OnMatchInGameAccess

public MatchInGameAccessEventHandler **OnMatchInGameAccess**;

## 전달인자

| Value | Type                        | Description        |
| :---- | :-------------------------- | :----------------- |
| args  | MatchInGameSessionEventArgs | 접속한 유저의 전적 |

## MatchInGameSessionEventArgs

| Value      | Type                                            | Description                            |
| :--------- | :---------------------------------------------- | :------------------------------------- |
| ErrInfo    | ErrorCode                                       | 에러 정보                              |
| Reason     | string                                          | 에러 정보                              |
| GameRecord | [MatchUserGameRecord](/sdk-docs/backend/match/argument) | 세션의 정보(세션 ID, 닉네임, 전적 등) |

## 설명

유저가 게임방에 입장할 때마다 호출되는 이벤트입니다.  

- 입장한 유저(자기 자신)에게 호출됩니다.  
- 이미 게임방에 접속해있던 모든 유저에게 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

### 이벤트 호출 규칙

**입장한 유저의 정보**가 GameRecord에 포함되어 있습니다.  

- A, B, C의 유저가 있을 때
- A가 방에 접속하면 A의 GameRecord가 포함된 OnMatchInGameAccess 이벤트 핸들러가 호출됩니다.  
- 방에 A가 접속해있고, B가 접속했을 때 A와 B는 B의 GameRecord가 포함된 OnMatchInGameAccess 이벤트 핸들러를 호출 받습니다.  
- 이후 C가 방에 입장하면 A, B, C는 각각 C의 GameRecord가 포함된 OnMatchInGameAccess 이벤트 핸들러를 호출 받습니다.  

## Example

```js
BackndBase.Match.OnMatchInGameAccess = (MatchInGameSessionEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 인게임 룸 접속에 성공한 경우 **  
ErrInfo : ErrorCode.Success  
GameRecord : "접속한 유저의 전적"

** 매칭된 룸이 아닌 다른 방에 접속을 시도한 경우 **  
ErrInfo : ErrorCode.Match_InGame_AuthroizeFailed  
Reason : Internal server error.  

** 룸 접속에 실패한 경우 **  
ErrInfo : ErrorCode.Exception  
Reason : "실패에 대한 이유"
