---
sidebar_label: 길드 가입 요청 거절
description: "길드 가입 요청 거절"
---

# RejectApplicant

public Task< RequestResult > **RejectApplicantAsync**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                        |
| ----------- | ------ | ---------------------------------- |
| gamerIndate | string | 길드 가입을 거절할 게이머의 indate |

## 설명

길드 가입 요청한 회원 중 해당 gamerIndate를 가진 회원의 가입을 거절합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.RejectApplicantAsync("gamerIndate");
```

### Callback 방식

```js
BackndGuild.Instance.RejectApplicant("gamerIndate", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**거절에 성공한 경우**  
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

