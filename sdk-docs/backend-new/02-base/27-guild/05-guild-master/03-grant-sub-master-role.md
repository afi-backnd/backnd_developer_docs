---
sidebar_label: 부 길드 마스터 지정
description: "부 길드 마스터 지정"
---

# AppointViceMaster

public Task< RequestResult > **AppointViceMasterAsync**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                             |
| ----------- | ------ | --------------------------------------- |
| gamerIndate | string | 부 길드 마스터로 지정할 게이머의 indate |

## 설명

길드 마스터가 부 길드 마스터를 지명합니다.  
부 길드 마스터의 인원수는 제한 없습니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.AppointViceMasterAsync("gamerIndate");
```

### Callback 방식

```js
BackndGuild.Instance.AppointViceMaster("gamerIndate", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**지명에 성공한 경우**  
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
