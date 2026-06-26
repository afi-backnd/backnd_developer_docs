---
sidebar_label: "외부 로그인 연동 계정 변경"
---

# ChangeLinkedProvider
public Task&lt;RequestResult&gt; **ChangeLinkedProviderAsync**(string **authToken**, AuthProvider **provider**);  

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| authToken     | string | 외부 로그인 인증 token |
| provider      | AuthProvider(enum) | 외부 로그인 제공자의 종류.<br />`AuthProvider.Google`<br />`AuthProvider.Facebook`<br/>`AuthProvider.Apple`<br />`AuthProvider.Steam`<br />`AuthProvider.GPGS2`<br />`AuthProvider.Line` |


## 설명
뒤끝 SDK에서 외부 계정과 연동한 계정을 다른 외부 계정 연동으로 변경하는 기능을 제공합니다.  
외부 로그인 계정으로 연동한 유저가 로그인을 진행한 후에 ChangeLinkedProvider 함수를 호출하면 다른 외부 계정 연동으로 전환이 가능합니다.  
가입되어 있지 않은 상태의 계정으로만 외부 계정 전환이 가능합니다.  
전환 후에는 새로 연결된 외부 계정으로 로그인하여 기존 게임 데이터를 사용할 수 있습니다.  
기존에 사용하던 외부 계정으로 로그인 하시면 새로운 계정이 생성됩니다.  
ChangeLinkedProvider 함수에 필요한 **authToken과 provider**은 [외부 로그인 인증 문서](https://docs.thebackend.io/sdk-docs/backend/base/user/federation/signup-and-login)에 더 자세하게 설명되어 있습니다.  


## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.ChangeLinkedProviderAsync("authToken", AuthProvider.Google);
if (reqResult.IsSuccess())
{
    Debug.Log("로그인 타입 전환에 성공했습니다");
}
```

### Callback 방식
```js
BackndAuth.Instance.ChangeLinkedProvider("authToken", AuthProvider.Google, callback =>
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

**customLogin 하지 않은 상황에서 시도한 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined access_token, access_token을(를) 확인할 수 없습니다

**외부 계정 전환에 사용되는 토큰값이 null 로 전송된 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined federation_access_token, federation_access_token을(를) 확인할 수 없습니다

**이미 외부 계정으로 가입된 계정에 커스텀 아이디 변경을 시도한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated federationId, 중복된 federationId 입니다

