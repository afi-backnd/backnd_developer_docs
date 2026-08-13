---
sidebar_label: "초대 수락/거절 이벤트"
description: "OnMatchMakingRoomInviteResponse"
---

# OnMatchMakingRoomInviteResponse

public MatchMakingResponseInviteEventHandler **OnMatchMakingRoomInviteResponse**;

## 전달인자

| Value | Type                            | Description         |
| :---- | :------------------------------ | :------------------ |
| args  | MatchMakingInteractionEventArgs | 초대 수락/거절 결과 |

## args 타입 명

| Value   | Type      | Description      |
| :------ | :-------- | :--------------- |
| ErrInfo | ErrorCode | 에러 정보        |
| Reason  | string    | 에러에 대한 설명 |

## 설명

다른 유저가 보낸 초대를 수락/거절했을 때 호출되는 이벤트입니다.  

- 초대를 수락/거절 한 유저에게만 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnMatchMakingRoomInviteResponse = (MatchMakingInteractionEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 초대 수락/거절 성공 **  
ErrInfo : ErrorCode.Success
