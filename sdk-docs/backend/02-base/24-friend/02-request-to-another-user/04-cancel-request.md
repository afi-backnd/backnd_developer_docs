---
sidebar_label: "보낸 친구 요청 취소"
description: "RevokeSentRequest"
---

# RevokeSentRequest

public BackendReturnObject **RevokeSentRequest**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                               |
| ----------- | ------ | ----------------------------------------- |
| gamerIndate | string | 친구 요청 철회하고자 하는 게이머의 indate |

## 설명

친구 요청을 받은 사람이 수락/거절하기 전, 친구 요청을 철회(취소)합니다.  

## Example

### 동기

```js
Backend.Friend.RevokeSentRequest("2020-02-11T01:08:12.603Z");
```

### 비동기

```js
Backend.Friend.RevokeSentRequest("2020-02-11T01:08:12.603Z", (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Friend.RevokeSentRequest, "2020-02-11T01:08:12.603Z", (callback) => {
    // 이후 처리
});
```

## ReturnCase

### Success cases

**철회에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**해당 유저에게 친구 신청을 하지 않았을 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : requestedGamer not found, requestedGamer을(를) 찾을 수 없습니다
