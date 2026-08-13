---
sidebar_label: "매칭 서버 접속 이벤트"
description: "OnJoinMatchMakingServer"
---

# OnJoinMatchMakingServer

public JoinChannelEventHandler **OnJoinMatchMakingServer**;

## 전달인자

| Value | Type                 | Description                   |
| :---- | :------------------- | :---------------------------- |
| args  | JoinChannelEventArgs | 채팅 서버 접속 성공/실패 여부 |

## JoinChannelEventArgs

| Value       | Type        | Description    |
| :---------- | :---------- | :------------- |
| ErrInfo     | ErrorInfo   | 에러 정보      |
| SessionInfo | SessionInfo | 접속 세션 정보 |

## 설명

매칭 서버에 접속을 성공/실패했을 때 호출되는 이벤트입니다.  
**해당 이벤트가 성공으로 호출되어야 매칭 서버에 접속 성공한 것입니다.**  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnJoinMatchMakingServer = (JoinChannelEventArgs args) =>
{
    // TODO
};
```

## ArgumentCase

** 매칭 서버 접속에 성공한 경우 **  
ErrInfo : ErrorInfo.Success

** 매칭 서버 접속에 실패한 경우**  
ErrInfo.Category : Exception  
ErrInfo.Reason : 접속 실패에 대한 이유
