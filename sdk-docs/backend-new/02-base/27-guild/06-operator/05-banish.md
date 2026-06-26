---
sidebar_label: 길드원 추방
description: "길드원 추방"
---

# KickMember

public Task< RequestResult > **KickMemberAsync**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                       |
| ----------- | ------ | --------------------------------- |
| gamerIndate | string | 길드에서 추방시킬 게이머의 indate |

## 설명

길드 마스터는 전체 멤버를 추방 가능, 부 길드 마스터는 길드 마스터와 부 길드 마스터를 제외한 모든 회원을 추방 가능합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.KickMemberAsync("gamerIndate");
```

### Callback 방식

```js
BackndGuild.Instance.KickMember("gamerIndate", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**추방에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

** 부 길드 마스터가 다른 부 길드 마스터를 추방 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden expelViceMaster, 금지된 expelViceMaster

**권한이 없는 유저가 추방 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden expelMaster, 금지된 expelMaster


**해당 gamerIndate가 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : requestedGamer not found, requestedGamer을(를) 찾을 수 없습니다
