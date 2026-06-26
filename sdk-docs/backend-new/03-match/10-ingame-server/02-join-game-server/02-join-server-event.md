---
sidebar_label: 인게임 서버 접속 이벤트
---

# OnSessionJoinInServer

public JoinChannelEventHandler **OnSessionJoinInServer**;

## 전달인자

| Value | Type                 | Description                     |
| :---- | :------------------- | :------------------------------ |
| args  | JoinChannelEventArgs | 인게임 서버 접속 성공/실패 여부 |

## JoinChannelEventArgs

| Value       | Type        | Description    |
| :---------- | :---------- | :------------- |
| ErrInfo     | ErrorInfo   | 에러 정보      |
| SessionInfo | SessionInfo | 접속 세션 정보 |

## 설명

인게임 서버에 접속을 성공/실패했을 때 호출되는 이벤트입니다.  

- 해당 이벤트가 성공으로 호출돼야 인게임 서버에 접속된 것입니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
BackndBase.Match.OnSessionJoinInServer += (args) => {
  // TODO
};
```

## ArgumentCase

** 인게임 서버 접속에 성공한 경우 **  
ErrInfo : ErrorInfo.Success

** 인게임 서버 접속에 실패한 경우 **  
ErrInfo.Category : ErrorCode.Exception  
ErrInfo.Reason : 접속 실패에 대한 이유

** 자기 자신이 인게임 서버 재접속에 성공한 경우 **  
ErrInfo.Category : ErrorCode.Success  
ErrInfo.Detail : ErrorCode.NetworkOnline  
ErrInfo.Reason : Reconnect Success

**비정상적인 방법으로 재접속을 시도하거나 서버에 이전 세션 접속 기록이 남아있어 접속에 실패한 경우**  
ErrInfo : ErrorCode.AuthenticationFailed  
Reason : Invalid rars operation(0).  
