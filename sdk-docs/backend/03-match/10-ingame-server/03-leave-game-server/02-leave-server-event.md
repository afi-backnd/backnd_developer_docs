---
sidebar_label: 인게임 서버 접속 종료 이벤트
---

# OnLeaveInGameServer

public MatchInGameLeaveSessionEventHandler **OnLeaveInGameServer**;

## 전달인자

| Value | Type                        | Description         |
| :---- | :-------------------------- | :------------------ |
| args  | MatchInGameSessionEventArgs | 접속 종료 에러 정보 |

## MatchInGameSessionEventArgs

| Value      | Type                                            | Description                            |
| :--------- | :---------------------------------------------- | :------------------------------------- |
| ErrInfo    | ErrorCode                                       | 에러 정보                              |
| Reason     | string                                          | 에러 정보                              |
| GameRecord | [MatchUserGameRecord](/sdk-docs/backend/match/argument) | 세션의 정보(세션 ID, 닉네임, 전적 등) |

## 설명

인게임 서버와 접속이 종료되었을 때 호출되는 이벤트입니다.  
아래의 경우 해당 이벤트가 호출될 수 있습니다.  

- 클라이언트에서 명시적으로 서버와 접속을 종료한 경우
- 게임방에서 게임이 종료되고 결과 종합이 완료된 경우
- 서버에서 클라이언트와 연결을 끊었을 경우
  - 매칭이 된 후 1분 안에 모든 유저가 게임방에 접속하지 않았을 때
  - 비정상적인 이유로 서버에서 접속을 종료했을 때
- 유저가 재접속에 실패한 경우

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
//example
Backend.Match.OnLeaveInGameServer = (MatchInGameSessionEventArgs args) => {
    // TODO
};
```

## ArgumentCase

**정상적으로 게임이 완료되고, 서버에서 접속을 끊었을 경우**  
ErrInfo : ErrorCode.Success

**게임방이 폐기되고, 서버에서 접속을 끊었을 경우**  
ErrInfo : ErrorCode.Success

**게임 결과 종합에 실패하여 게임이 무효 처리된 후, 서버에서 접속을 끊었을 경우**  
ErrInfo : ErrorCode.Success

**비정상적인 에러로 서버에서 접속을 종료했을 때**  
ErrInfo : ErrorCode.Exception  
Reason : "에러에 대한 이유"

**재접속에 실패했을 때**  
ErrInfo : ErrorCode.Exception  
Reason : Fail To Reconnect

**비정상적인 방법으로 재접속을 시도할 때**  
ErrInfo : ErrorCode.AuthenticationFailed  
Reason : Invalid rars operation(0).  

**데이터 Read 중 유니티 TLS 오류로 SSL 스트림이 수립되지 않은 경우.**  
ErrInfo : ErrorCode.Exception  
Reason : System.Security.Authentication.AuthenticationException: A call to SSPI failed, see inner exception.  

> 이 경우 SDK에서 서버와 자동으로 재접속을 시도합니다.  
