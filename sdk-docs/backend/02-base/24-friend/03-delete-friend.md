---
sidebar_label: 친구 삭제
---

# BreakFriend

public BackendReturnObject **BreakFriend**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description               |
| ----------- | ------ | ------------------------- |
| gamerIndate | string | 삭제할 친구의 gamerIndate |

## 설명

해당 gamerIndate의 유저를 친구 목록에서 삭제합니다.  

## Example

### 동기

```js
Backend.Friend.BreakFriend("2020-12-10T08:22:30.891Z");
```

### 비동기

```js
Backend.Friend.BreakFriend("2020-12-10T08:22:30.891Z", (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Friend.BreakFriend, "2020-12-10T08:22:30.891Z", (callback) => {
    // 이후 처리
});
```

## ReturnCase

### Success cases

**삭제에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**gamerIndate가 올바르지 않는 경우/해당 유저가 친구가 아닌 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : friendGamer not found, friendGamer을(를) 찾을 수 없습니다
