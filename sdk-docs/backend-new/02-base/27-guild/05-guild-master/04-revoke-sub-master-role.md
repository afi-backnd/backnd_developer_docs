---
sidebar_label: 부 길드 마스터 해제
description: "부 길드 마스터 해제"
---

# DismissViceMaster

public Task< RequestResult > **DismissViceMasterAsync**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                             |
| ----------- | ------ | --------------------------------------- |
| gamerIndate | string | 부 길드 마스터를 해제할 게이머의 indate |

## 설명

길드 마스터가 해당 gamerIndate를 가진 부 길드 마스터를 해제합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.DismissViceMasterAsync("gamerIndate");
```

### Callback 방식

```js
BackndGuild.Instance.DismissViceMaster("gamerIndate", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**해제에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**마스터 이외의 길드원이 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden changeViceMaster, 금지된 changeViceMaster

**길드에 없는 유저일 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guildMember not found, guildMember을(를) 찾을 수 없습니다

**해당 유저가 부 길드 마스터가 아닌 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : viceMaster not found, viceMaster을(를) 찾을 수 없습니다  

