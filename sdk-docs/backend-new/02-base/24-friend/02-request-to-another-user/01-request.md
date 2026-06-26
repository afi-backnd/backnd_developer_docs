---
sidebar_label: 친구 요청
---

# SendFriendRequest

public Task< RequestResult > **SendFriendRequestAsync**(string **gamerIndate**);

## 파라미터

| Value       | Type   | Description                               |
| ----------- | ------ | ----------------------------------------- |
| gamerIndate | string | 친구 요청을 보내고자 하는 게이머의 indate |

## 설명

해당 gamerIndate를 가진 유저에게 친구 요청을 합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndFriend.Instance.SendFriendRequestAsync("2020-02-11T01:08:12.603Z");
```

### Callback 방식

```js
BackndFriend.Instance.SendFriendRequest("2020-02-11T01:08:12.603Z", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**친구 요청에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**뒤끝 콘솔의 소셜관리 메뉴에서 친구 최대보유수 설정값이 0인 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden friend, 금지된 friend

**이미 친구 요청한 사람에게 다시 요청한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated existReqeustFriendGamer, 중복된 existReqeustFriendGamer 입니다

**받는 사람의 request 가 꽉 찬 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : maxRequestFriend 사전 조건을 만족하지 않습니다.  

**보내는 사람의 request 가 꽉 찬 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : maxSendFriendRequest 사전 조건을 만족하지 않습니다.  


