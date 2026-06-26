---
sidebar_label: 게임 결과 이벤트
---

# OnMatchResult

public MatchResultEventHandler **OnMatchResult**;

## 전달인자

| Value | Type                 | Description              |
| :---- | :------------------- | :----------------------- |
| args  | MatchResultEventArgs | 결과 종합 성공/실패 여부 |

## MatchResultEventArgs

| Value   | Type      | Description |
| :------ | :-------- | :---------- |
| ErrInfo | ErrorCode | 에러 정보   |
| Reason  | string    | 에러 이유   |

## 설명

게임방의 게임이 종료되었을 때 호출되는 이벤트입니다.  

- 서버에서 결과 종합이 끝난 후 모든 클라이언트에서 호출되는 이벤트입니다.  
- 게임 시간이 초과되거나, 모든 클라이언트가 게임에 접속되지 못해 게임방이 파기되는 등 게임 자체가 끝나는 경우에도 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
BackndBase.Match.OnMatchResult = (MatchResultEventArgs args) => {
    // TODO
};
```

## ArgumentCase

**결과 종합 성공**  
ErrInfo : ErrorCode.Success

**결과 종합 실패(서버에서 결과 종합을 실패한 경우)**  
ErrInfo : ErrorCode.Exception  
Reason : 결과 종합 실패 이유(혹은 FDM 코드 리턴)

**게임 시작 실패(룸 생성 후 모든 유저가 게임에 접속하지 않은 경우)**  
ErrInfo : Match_InGame_Timeout  
Reason : Some gamers are not connected.(0)

**결과 종합 실패(모든 유저가 결괏값을 서버로 전송하지 않은 경우)**  
ErrInfo : Exception  
Reason : error: Success, status: 400, reason: {"errorCode":"BadParameterException","message":"bad headCount, 잘못된 headCount 입니다","statusCode":400}

> 콘솔에서 **결과 처리를 기본 모드로 설정**한 경우
> 한 유저가 결과 종합 메시지를 서버로 전송한 후 15초 이내에 모든 유저가 결과 종합 메시지를 서버로 전송하지 않은 경우 해당 에러가 발생할 수 있습니다.  
> 모든 유저가 게임 결과를 보내지 않은 경우 **해당 게임은 무효 처리** 됩니다.  

**결과 종합 실패(게임에 참여한 유저의 수와 결과에 포함된 유저 수가 다른 경우)**  
ErrInfo : Exception  
Reason : error: Success, status: 400, reason: {"errorCode":"BadParameterException","message":"bad headCount, 잘못된 headCount 입니다","statusCode":400}

**결과 종합 실패(결과에 포함되어 있는 승/패 유저 리스트와 실제 팀 유저들이 일치하지 않는 경우)**  
ErrInfo : Exception  
Reason : error: Success, status: 400, reason: {"errorCode":"BadParameterException","message":"bad invalid team infomation, 잘못된 invalid team infomation 입니다","statusCode":400}

**게임 시간 초과(콘솔에서 설정한 매치 제한 시간을 초과한 경우)**  
ErrInfo : Match_InGame_Timeout  
Reason : Timeout

**올바르지 않은 결괏값을 서버로 보낸 경우(결괏값을 보내지 않거나, 일부 유저만 결괏값에 포함시키거나 등...)**  
ErrInfo : ErrorCode.Exception  
Reason : FDM: "에러FDM 코드"
