---
sidebar_label: 유저 입장 이벤트 이벤트
---

# OnMatchMakingRoomJoin

public MatchMakingJoinUserInRoomEventHandler OnMatchMakingRoomJoin;

## 전달인자

| Value | Type                                | Description               |
| :---- | :---------------------------------- | :------------------------ |
| args  | MatchMakingGamerInfoInRoomEventArgs | 대기방에 입장한 유저 정보 |

## args 타입 명

| Value    | Type                                            | Description               |
| :------- | :---------------------------------------------- | :------------------------ |
| ErrInfo  | ErrorCode                                       | 에러 정보                 |
| Reason   | string                                          | 에러에 대한 설명          |
| UserInfo | [MatchMakingUserInfo](/sdk-docs/backend/match/argument/#matchmakinguserinfo) | 대기방에 입장한 유저 정보 |

## 설명

대기방에 유저가 입장했을 때 호출되는 이벤트입니다.  

- 유저가 입장할 때마다 대기방에 존재하는 모든 유저에게 호출됩니다.  
- 입장한 유저에게도 호출됩니다.  
- **대기방을 생성했을 때는 호출되지 않습니다.**  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnMatchMakingRoomJoin = (MatchMakingGamerInfoInRoomEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 대기방에 입장했을 때 **  
ErrInfo : ErrorCode.Success  
UserInfos : 대기방에 입장한 유저 정보
