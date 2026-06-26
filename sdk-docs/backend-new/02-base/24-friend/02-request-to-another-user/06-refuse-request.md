---
sidebar_label: 받은 친구 요청 거절
description: "받은 친구 요청 거절"
---

# RejectFriendRequest
public Task< RequestResult > **RejectFriendRequestAsync**(string **gamerIndate**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| gamerIndate | string  | 친구 요청을 거절하고자 하는 게이머의 indate |

## 설명
친구 요청을 거절합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndFriend.Instance.RejectFriendRequestAsync("2020-02-11T01:08:12.603Z");
```

### Callback 방식
```js
BackndFriend.Instance.RejectFriendRequest("2020-02-11T01:08:12.603Z", (callback) => 
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**거절에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases
**해당 gamerIndate가 친구가 아닐 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : requestFriend not found, requestFriend을(를) 찾을 수 없습니다
