---
sidebar_label: "길드명 변경"
sidebar_position: "1.1"
description: "UpdateGuildName"
---

# UpdateGuildName

public BackendReturnObject **UpdateGuildName**(string **guildName**);

## 파라미터

| Value     | Type   | Description         |
| --------- | ------ | ------------------- |
| guildName | string | 변경할 길드의 이름. |


## 설명

길드의 이름을 변경합니다.

## Example

### 동기

```js
Backend.Guild.UpdateGuildName("새로운 길드 이름");
```

### 비동기

```js
Backend.Guild.UpdateGuildName("새로운 길드 이름", (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.UpdateGuildName, "새로운 길드 이름", (callback) => {
    // 이후 처리
  });
```

## ReturnCase

### Success cases

**사용한 경우**  
statusCode : 204  
message : Success  

### Error cases

**마스터 이외의 길드원이 사용 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : no permission to change guild name because you are not the master 사전 조건을 만족하지 않습니다.

**길드이름 형식 오류**  
statusCode : 412  
errorCode : PreconditionFailed  
message : bad guildName, 잘못된 guildName 입니다.

**길드에 속해있지 않은 사람이 사용 시도한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : subscribed guild 사전 조건을 만족하지 않습니다.
