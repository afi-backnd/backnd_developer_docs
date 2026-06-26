---
sidebar_label: 커스텀 -> 외부 로그인 연동
---

# LinkWithProvider
public Task&lt;RequestResult&gt; **LinkWithProviderAsync**(string **authToken**, AuthProvider **provider**);  

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| authToken     | string | 외부 로그인 인증 token |
| provider      | AuthProvider(enum) | 외부 로그인 제공자의 종류.<br />`AuthProvider.Google`<br />`AuthProvider.Facebook`<br/>`AuthProvider.Apple`<br />`AuthProvider.Steam`<br />`AuthProvider.GPGS2`<br />`AuthProvider.Line` |

## 설명
뒤끝 SDK에서 커스텀으로 가입한 아이디를 외부 계정과 연동하는 기능을 제공합니다.  
커스텀 유저가 로그인을 진행한 후에 LinkWithProvider 함수를 호출하면 외부 계정 로그인과 연동 가능합니다.  
LinkWithProvider 필요한 **authToken과 provider**은 [외부 로그인 인증 문서](https://docs.thebackend.io/sdk-docs/backend/base/user/federation/signup-and-login)에 더 자세하게 설명되어 있습니다.  
>  커스텀 계정을 외부 계정과 연동할 때 이메일 권한을 추가하지 않은 경우 회원 아이디는 커스텀 아이디로 표시됩니다.  


## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.LinkWithProviderAsync("authToken", AuthProvider.Google);
if (reqResult.IsSuccess())
{
    Debug.Log("로그인 타입 전환에 성공했습니다");
}
```

### Callback 방식
```js
BackndAuth.Instance.LinkWithProvider("authToken", AuthProvider.Google, callback =>
{
    if (callback.IsSuccess())
    {
        Debug.Log("로그인 타입 전환에 성공했습니다");
    }
});
```

## ReturnCase

### Success cases

**전환에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**이미 ChangeCustomToFederation 완료되었는데 다시 시도한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad type, 잘못된 type 입니다

**customLogin 하지 않은 상황에서 시도한 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined access_token, access_token을(를) 확인할 수 없습니다

**외부 계정 연동에 사용되는 토큰값이 null 로 전송된 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined federation_access_token, federation_access_token을(를) 확인할 수 없습니다

**이미 외부 계정으로 가입된 계정에 커스텀 아이디 변경을 시도한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated federationId, 중복된 federationId 입니다

