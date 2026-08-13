---
sidebar_label: "받은 친구 요청 수락"
description: "AcceptFriend"
---

# AcceptFriend

public BackendReturnObject **AcceptFriend**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                                 |
| ----------- | ------ | ------------------------------------------- |
| gamerIndate | string | 친구 요청을 수락하고자 하는 게이머의 indate |

## 설명

친구 요청을 수락합니다.  

## Example

### 동기

```js
Backend.Friend.AcceptFriend("2020-02-11T01:08:12.603Z");
```

### 비동기

```js
Backend.Friend.AcceptFriend("2020-02-11T01:08:12.603Z", (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Friend.AcceptFriend, "2020-02-11T01:08:12.603Z", (callback) => {
    // 이후 처리
});
```

## ReturnCase

### Success cases

**수락에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**요청한 사람의 friend list 가 꽉 찬 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : maxRequestedGamerFriend 사전 조건을 만족하지 않습니다.  

**요청받은 사람의 friend list 가 꽉 찬 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : maxGamerFriend 사전 조건을 만족하지 않습니다.  
