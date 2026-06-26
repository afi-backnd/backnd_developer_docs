---
sidebar_label: 대기방 퇴장 이벤트
---

# OnMatchMakingRoomLeave

public MatchMakingLeaveUserInRoomEventHandler **OnMatchMakingRoomLeave**;

## 전달인자

| Value | Type                                | Description           |
| :---- | :---------------------------------- | :-------------------- |
| args  | MatchMakingGamerInfoInRoomEventArgs | 대기방 퇴장 유저 정보 |

## MatchMakingGamerInfoInRoomEventArgs

| Value    | Type                                            | Description               |
| :------- | :---------------------------------------------- | :------------------------ |
| ErrInfo  | ErrorCode                                       | 에러 정보                 |
| Reason   | string                                          | 에러에 대한 설명          |
| UserInfo | [MatchMakingUserInfo](/sdk-docs/backend/match/argument/#matchmakinguserinfo) | 대기방에 퇴장한 유저 정보 |

## 설명

대기방에서 유저가 퇴장한 경우 호출되는 이벤트입니다.  

- 자기 자신이 퇴장한 경우 호출됩니다.  
- 다른 유저가 퇴장한 경우에도 호출됩니다.  
- 대기방에서 [강퇴](/sdk-docs/backend/match/server/kick-match-room/kick-user) 당한 경우에도 호출됩니다.  
- **방장이 대기방에서 퇴장하여 만료된 경우에는 아래와 같이 이벤트가 호출됩니다.**
  - 방장의 퇴장 이벤트는 호출됩니다.  
  - 자기 자신의 퇴장 이벤트는 호출되지 않습니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnMatchMakingRoomLeave = (MatchMakingGamerInfoInRoomEventArgs args) => {
    // TODO
};
```

## ArgumentCase

**나 혹은 다른 유저가 퇴장한 경우**  
ErrInfo : ErrorCode.Success  
UserInfo : 퇴장한 유저 정보

**매칭 진행 중 대기방에서 퇴장을 시도한 경우(퇴장 실패)**  
ErrInfo : ErrorCode.InvalidOperation  
Reason : In-progress matchmaking.  

**나 혹은 다른 유저가 강퇴당한 경우**  
ErrInfo : ErrorCode.Match_Making_KickedByOwner  
UserInfo : 강퇴당한 유저 정보
