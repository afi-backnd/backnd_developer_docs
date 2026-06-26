---
sidebar_label: 초대요청 송신 이벤트
---

# OnMatchMakingRoomInvite

public MatchMakingInviteUserEventHandler **OnMatchMakingRoomInvite**;

## 전달인자

| Value | Type                            | Description                |
| :---- | :------------------------------ | :------------------------- |
| args  | MatchMakingInteractionEventArgs | 유저 초대 성공 / 실패 여부 |

## MatchMakingInteractionEventArgs

| Value   | Type      | Description      |
| :------ | :-------- | :--------------- |
| ErrInfo | ErrorCode | 에러 정보        |
| Reason  | string    | 에러에 대한 설명 |

## 설명

유저를 초대했을 때 호출되는 이벤트 핸들러입니다.  

- **초대 요청 송신의 성공/실패 여부**를 확인할 수 있습니다.  
- 유저의 초대 수락/거절 여부는 확인할 수 없습니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnMatchMakingRoomInvite = (MatchMakingInteractionEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 유저 초대 메시지를 보내는데 성공한 경우 **  
ErrInfo : ErrorInfo.Success

** 대기방을 만들지 않고 유저를 초대한 경우 **  
ErrInfo : ErrorInfo.Match_Making_NotJoinedRoom  
Reason : You are not joined room.  

** 초대한 유저가 매칭 서버에 접속하지 않은 경우 **  
ErrInfo : ErrorInfo.Match_Making_NotFoundGamer  
Reason : Not found {nickName} gamer.  

** 이미 같은/다른 방에 입장해있는 유저를 초대했을 경우 **  
ErrInfo : Match_Making_AlreadyJoinedRoom  
Reason : {nickName} Gamer already joined the another room.  
