---
sidebar_label: "토큰 유효성 체크"
description: "IsAccessTokenAlive"
---

# IsAccessTokenAlive
public BackendReturnObject **IsAccessTokenAlive**();

## 설명
현재 유저가 갖고 있는 AccessToken이 뒤끝 통신을 위해 유효한지 확인합니다.  

## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.IsAccessTokenAlive();
if(bro.IsSuccess())
{
   Debug.Log("액세스 토큰이 살아있습니다");
}
```

### 비동기
```js
Backend.BMember.IsAccessTokenAlive((callback) =>
{
  if(callback.IsSuccess())
      Debug.Log("액세스 토큰이 살아있습니다");
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.IsAccessTokenAlive, (callback) =>
{
  if(callback.IsSuccess())
      Debug.Log("액세스 토큰이 살아있습니다");
});
```

## ReturnCase

### Success cases

**유효한 경우**  
statusCode : 204  
message : Success  

### Error cases

**기기 로컬에 액세스 토큰이 존재하지 않는데 토큰 로그인 시도를 한 경우**  
statusCode : 400  
errorCode : accessTokenError  
message : accessToken not exist  

**customLogin 하지 않은 상황에서 시도한 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined access_token, access_token을(를) 확인할 수 없습니다  

**유효하지 않는 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad accessToken, 잘못된 accessToken 입니다
