---
sidebar_label: 길드 가입 승인
description: "길드 가입 승인"
---

# ApproveApplicant

public Task< RequestResult > **ApproveApplicantAsync**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                        |
| ----------- | ------ | ---------------------------------- |
| gamerIndate | string | 길드 가입을 수락할 게이머의 indate |

## 설명

길드 가입 요청한 회원 중 해당 gamerIndate를 가진 회원의 가입을 승인합니다.  
최대 길드 인원은 100명입니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.ApproveApplicantAsync("gamerIndate");
```

### Callback 방식

```js
BackndGuild.Instance.ApproveApplicant("gamerIndate", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**수락한 경우**  
statusCode : 204  
message : Success  

### Error cases

**길드 마스터 혹은 운영진이 아닐 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden approveApplicant, 금지된 approveApplicant

**해당 gamerIndate가 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : requestedGamer not found, requestedGamer을(를) 찾을 수 없습니다

**수락하려는 회원이 이미 다른 길드에 속한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : JoinedGamer 사전 조건을 만족하지 않습니다.  

**길드원이 이미 100명 이상인 경우**  
statusCode : 429  
errorCode : Too Many Request  
message : guild member count 요청 횟수를 초과하였습니다.  

