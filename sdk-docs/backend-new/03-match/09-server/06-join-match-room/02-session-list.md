---
sidebar_label: 대기방 유저 리스트 이벤트
---

# OnMatchMakingRoomUserList

public MatchMakingUserListInRoomEventHandler **OnMatchMakingRoomUserList**;

## 전달인자

| Value | Type                                    | Description                     |
| :---- | :-------------------------------------- | :------------------------------ |
| args  | MatchMakingGamerInfoListInRoomEventArgs | 대기방에 존재하는 유저들의 정보 |

## args 타입 명

| Value     | Type                                                   | Description                                                   |
| :-------- | :----------------------------------------------------- | :------------------------------------------------------------ |
| ErrInfo   | ErrorCode                                              | 에러 정보                                                     |
| Reason    | string                                                 | 에러에 대한 설명                                              |
| UserInfos | List <[MatchMakingUserInfo](/sdk-docs/backend/match/argument/#matchmakinguserinfo)> | 대기방에 존재하는 유저들 리스트  
(입장한 유저 정보도 포함) |

## 설명

초대받은 유저가 대기방에 입장했을 때 호출되는 이벤트 핸들러입니다.  

- **대기방을 입장한 유저에게만 호출됩니다.**  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
BackndBase.Match.OnMatchMakingRoomUserList = (MatchMakingGamerInfoListInRoomEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 초대받은 대기방에 입장했을 때 **  
ErrInfo : ErrorCode.Success  
UserInfos : 대기방에 존재하는 모든 유저의 정보
