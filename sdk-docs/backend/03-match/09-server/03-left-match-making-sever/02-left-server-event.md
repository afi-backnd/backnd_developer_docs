---
sidebar_label: "매칭 서버 접속 종료 이벤트"
description: "OnLeaveMatchMakingServer"
---

# OnLeaveMatchMakingServer

public LeaveChannelEventHandler **OnLeaveMatchMakingServer**;

## 전달인자

| Value | Type                  | Description                   |
| :---- | :-------------------- | :---------------------------- |
| args  | LeaveChannelEventArgs | 채팅 서버 접속 종료/실패 여부 |

## LeaveChannelEventArgs

| Value       | Type        | Description    |
| :---------- | :---------- | :------------- |
| ErrInfo     | ErrorInfo   | 에러 정보      |
| SessionInfo | SessionInfo | 종료 세션 정보 |

## 설명

서버와 접속이 종료되었을 때 호출되는 이벤트입니다.  
아래의 경우 해당 이벤트가 호출될 수 있습니다.  

- 클라이언트에서 명시적으로 서버와 접속을 종료한 경우
- 콘솔에서 생성하지 않은 유형의 매칭을 신청하였을 경우
- 매칭 서버에서 비정상적인 이유로 클라이언트와 연결을 끊었을 경우

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnLeaveMatchMakingServer = (LeaveChannelEventArgs args) =>
{
    // TODO
};
```

## ArgumentCase

**매칭 서버와 정상적으로 접속에 종료한 경우**  
ErrInfo.Category : ErrorCode.Success  
ErrInfo.Detail : ErrorCode.DisconnectFromLocal

**매칭 서버에서 비정상적인 이유로 클라이언트와의 연결을 강제로 끊은 경우**  
ErrInfo.Category : ErrorCode.Exception  
ErrInfo.Detail : ErrorCode.DisconnectFromRemote

**콘솔에서 생성하지 않은 매치 타입 & 매치 유형으로 매칭을 신청했을 때**  
ErrInfo.Category : ErrorCode.DisconnectFromRemote  
ErrInfo.Detail : ErrorCode.Exception  
ErrInfo.Reason : boost::bad_format_string: format-string is ill-formed

**매치 서버와 클라이언트가 30초 이상 연결이 끊어진 경우  
(Poll 함수가 호출되지 않음, 게임이 백그라운드로 내려감 등)**  
ErrInfo.Category : ErrorCode.NetworkTimeout  
ErrInfo.Reason : Session Disconnect(0)
