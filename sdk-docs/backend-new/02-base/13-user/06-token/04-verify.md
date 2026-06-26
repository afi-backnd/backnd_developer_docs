---
sidebar_label: 토큰 유효성 체크
description: "토큰 유효성 체크"
---

# ValidateAccessToken
public Task&lt;RequestResult&gt; **ValidateAccessTokenAsync**();

## 설명
현재 유저가 갖고 있는 AccessToken이 뒤끝 통신을 위해 유효한지 확인합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.ValidateAccessTokenAsync();
if (reqResult.IsSuccess())
{
    Debug.Log("액세스 토큰이 살아있습니다");
}
```

### Callback 방식
```js
BackndAuth.Instance.ValidateAccessToken((callback) =>
{
    if (callback.IsSuccess())
    {
        Debug.Log("액세스 토큰이 살아있습니다");
    }   
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
