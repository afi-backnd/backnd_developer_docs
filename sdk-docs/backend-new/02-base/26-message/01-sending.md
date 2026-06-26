---
sidebar_label: 쪽지 보내기
---

# SendMessage

public Task< RequestResult > **SendMessageAsync**(string **gamerIndate**, string **contents**);

## 파라미터

| Value       | Type   | Description                 |
| ----------- | ------ | --------------------------- |
| gamerIndate | string | 쪽지를 받을 게이머의 indate |
| contents    | string | 쪽지 내용                   |

## 설명

해당 gamerIndate의 유저에게 쪽지를 보냅니다.  
쪽지 contents의 글자 수는 콘솔에서 설정한 글자 수만큼만 보낼 수 있습니다.  

> 각각의 보낸 쪽지 리스트와 받은 쪽지 리스트는 **[콘솔 - 프로젝트 설정 - 소셜 설정]**에서 설정한 최대 보유수만큼만 보유 가능합니다.  
> 쪽지 기능은 게이머 닉네임이 존재해야만 사용 가능합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndMessage.Instance.SendMessageAsync("2020-02-11T01:08:12.603Z", "오늘 하루도 좋은 하루 보내");
```

### Callback 방식

```js
BackndMessage.Instance.SendMessage("2020-02-11T01:08:12.603Z", "오늘 하루도 좋은 하루 보내", (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**전송에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**보내는 사람과 받는 사람이 같은 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad Sender and recipient can not be the same., 잘못된 Sender and recipient can not be the same. 입니다

**보내는 사람의 닉네임이 없는 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad senderNickname, 잘못된 senderNickname 입니다

**쪽지 최대 보유수가 0인 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden send message 0, 금지된 send message 0

**받는 사람의 메시지가 꽉 찬 경우**  
statusCode : 405  
errorCode : MethodNotAllowedParameterException  
message : MethodNotAllowed The recipients message is full., 이용할 수 없는 The recipients message is full. 입니다

**contents 글자 수가 콘솔에서 설정한 글자 수보다 큰 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : content length 사전 조건을 만족하지 않습니다.  

