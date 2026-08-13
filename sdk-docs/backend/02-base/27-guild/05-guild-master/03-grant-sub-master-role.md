---
sidebar_label: "부 길드 마스터 지정"
description: "NominateViceMasterV3"
---

# NominateViceMasterV3

public BackendReturnObject **NominateViceMasterV3**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                             |
| ----------- | ------ | --------------------------------------- |
| gamerIndate | string | 부 길드 마스터로 지정할 게이머의 indate |

## 설명

길드 마스터가 부 길드 마스터를 지명합니다.  
부 길드 마스터의 인원수는 제한 없습니다.  

## Example

### 동기

```js
Backend.Guild.NominateViceMasterV3("gamerIndate");
```

### 비동기

```js
Backend.Guild.NominateViceMasterV3("gamerIndate", (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.NominateViceMasterV3,"gamerIndate", (callback) => {
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
