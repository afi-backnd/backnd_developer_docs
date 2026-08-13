---
sidebar_label: "대기방 생성 이벤트"
description: "OnMatchMakingRoomCreate"
---

# OnMatchMakingRoomCreate

public MatchMakingCreateRoomEventHandler **OnMatchMakingRoomCreate**;

## 전달인자

| Value | Type                            | Description            |
| :---- | :------------------------------ | :--------------------- |
| args  | MatchMakingInteractionEventArgs | 방 생성/성공 실패 정보 |

## MatchMakingInteractionEventArgs

| Value   | Type      | Description      |
| :------ | :-------- | :--------------- |
| ErrInfo | ErrorCode | 에러 정보        |
| Reason  | string    | 에러에 대한 설명 |

## 설명

대기방을 생성하였을 때 호출되는 이벤트 핸들러입니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnMatchMakingRoomCreate = (MatchMakingInteractionEventArgs args) =>
{
    // TODO
};
```

## ArgumentCase

** 대기방 생성에 성공한 경우 **  
ErrInfo : ErrorInfo.Success
