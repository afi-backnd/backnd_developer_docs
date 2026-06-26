---
sidebar_label: 길드 마스터 권한 위임
---

# NominateMasterV3

public BackendReturnObject **NominateMasterV3**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                          |
| ----------- | ------ | ------------------------------------ |
| gamerIndate | string | 길드 마스터로 지정할 게이머의 indate |

## 설명

현재 길드 마스터가 길드 멤버에게 길드 마스터 권한을 위임합니다.  
길드 마스터는 길드원으로 권한이 변경됩니다.  

## Example

### 동기

```js
Backend.Guild.NominateMasterV3("gamerIndate");
```

### 비동기

```js
Backend.Guild.NominateMasterV3("gamerIndate", (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.NominateMasterV3, "gamerIndate", (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**위임에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**마스터 이외의 길드원이 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden changeMaster, 금지된 changeMaster

**길드에 없는 유저일 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guildMember not found, guildMember을(를) 찾을 수 없습니다
