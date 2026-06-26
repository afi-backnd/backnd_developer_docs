---
sidebar_label: 로그아웃
---

# SignOut
public Task&lt;RequestResult&gt; **SignOutAsync**();


## 설명
서버에서 로그아웃합니다.  
로그아웃을 하게 되면 서버에서 뒤끝 access_token과 refresh_token을 삭제하는 작업이 발생됩니다.  
따라서 기존에 받았던 access_token과 refresh_token는 더 이상 사용이 불가합니다.  
* 새로운 access_token과 refresh_token을 발급받기 위해서는 **AuthorizeFederation / CustomSignUp / CustomLogin** 과정을 거쳐야 합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.SignOutAsync();
```

### Callback 방식
```js
Backend.BMember.SignOut((callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**로그아웃에 성공한 경우**  
statusCode : 204  
message : Success  

