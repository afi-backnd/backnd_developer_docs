---
sidebar_label: 초대요청 수신 이벤트
---

# OnMatchMakingRoomSomeoneInvited

public MatchMakingInvitedRoomEventHandler **OnMatchMakingRoomSomeoneInvited**;

## 전달인자

| Value | Type                            | Description |
| :---- | :------------------------------ | :---------- |
| args  | MatchMakingInvitedRoomEventArgs | 초대 정보   |

## MatchMakingInvitedRoomEventArgs

| Value          | Type                                            | Description                                |
| :------------- | :---------------------------------------------- | :----------------------------------------- |
| ErrInfo        | ErrorCode                                       | 에러 정보                                  |
| Reason         | string                                          | 에러에 대한 설명                           |
| RoomId         | SessionId                                       | 입장할 룸 ID(대기방의 고유한 룸 ID)       |
| RoomToken      | string                                          | 입장할 룸 토큰(초대할 때마다 변경됩니다.) |
| InviteUserInfo | [MatchMakingUserInfo](/sdk-docs/backend/match/argument/#matchmakinguserinfo) | 초대한 유저 정보                           |

## 설명

다른 유저가 나를 대기방으로 초대했을 때 호출되는 이벤트입니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
BackndBase.Match.OnMatchMakingRoomSomeoneInvited += (args) => {
  // TODO
};
```

## ArgumentCase

** 유저한테 초대가 왔을 때 **  
ErrInfo : ErrorInfo.Success  
RoomId : 룸 ID  
RoomToken : 룸 토큰  
InviteUserInfo : 초대한 유저 정보(세션 ID & 닉네임)
