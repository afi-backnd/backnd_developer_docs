---
sidebar_label: 유저 강퇴 이벤트
---

# OnMatchMakingRoomKick

public MatchMakingKickUserEventHandler OnMatchMakingRoomKick;

## 전달인자

| Value | Type                            | Description           |
| :---- | :------------------------------ | :-------------------- |
| args  | MatchMakingInteractionEventArgs | 강퇴 성공 / 실패 여부 |

## MatchMakingInteractionEventArgs

| Value   | Type      | Description      |
| :------ | :-------- | :--------------- |
| ErrInfo | ErrorCode | 에러 정보        |
| Reason  | string    | 에러에 대한 설명 |

## 설명

유저 강퇴 함수를 호출했을 때 **방장에게만 호출**되는 이벤트입니다.  

- 강퇴당한 유저의 정보는 대기방에 존재하는 모든 유저에게 [OnMatchMakingRoomLeave](/sdk-docs/backend/match/server/leave-match-room/leave-room-event) 이벤트로 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnMatchMakingRoomKick = (MatchMakingInteractionEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 강퇴에 성공했을 때 **  
ErrInfo : ErrorCode.Success

** 자기 자신을 강퇴했을 때 **  
ErrInfo : ErrorCode.Match_Making_NotFoundGamer  
Reason : Invalid {nickName} gamer.  

** 대기방에 존재하지 않는 유저를 강퇴했을 때 **  
ErrInfo : ErrorCode.Match_Making_NotFoundGamer  
Reason : Invalid {nickName} gamer.  

** 방장이 아닌데 강퇴를 시도했을 때 **  
ErrInfo : ErrorCode.InvalidOperation  
Reason : You are not owner of room.  
