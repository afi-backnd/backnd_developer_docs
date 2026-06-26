---
sidebar_label: 길드 탈퇴
description: "길드 탈퇴"
---

# LeaveGuild

public Task< RequestResult > **LeaveGuildAsync**();

## 설명

현재 가입되어 있는 길드를 탈퇴합니다.  
길드 마스터의 경우, 길드원이 하나도 없는 경우 길드 탈퇴가 가능합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.LeaveGuildAsync();
```

### Callback 방식

```js
BackndGuild.Instance.LeaveGuild((callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**탈퇴에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**유저가 길드 마스터인데 멤버가 남아있는 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : memberExist 사전 조건을 만족하지 않습니다.  

**길드에 속하지 않은 유저가 탈퇴 시도한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : subscribed guild 사전 조건을 만족하지 않습니다.  
