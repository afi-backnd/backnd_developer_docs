---
sidebar_label: 대기방 삭제 이벤트
---

# OnMatchMakingRoomDestory

MatchMakingDestoryRoomEventHandler **OnMatchMakingRoomDestory**;

## 전달인자

| Value | Type                            | Description |
| :---- | :------------------------------ | :---------- |
| args  | MatchMakingInteractionEventArgs | 만료 정보   |

## MatchMakingInteractionEventArgs

| Value   | Type      | Description      |
| :------ | :-------- | :--------------- |
| ErrInfo | ErrorCode | 에러 정보        |
| Reason  | string    | 에러에 대한 설명 |

## 설명

방장이 대기방에서 퇴장했을 때 호출되는 이벤트 핸들러입니다.  

- 방장이 대기방에서 퇴장하면 해당 대기방은 삭제됩니다.  
- 삭제된 대기방에 입장 중이던 모든 유저에게 해당 이벤트가 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
BackndBase.Match.OnMatchMakingRoomDestory = (MatchMakingInteractionEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 대기방이 삭제된 경우 **  
ErrInfo : ErrorCode.Success
