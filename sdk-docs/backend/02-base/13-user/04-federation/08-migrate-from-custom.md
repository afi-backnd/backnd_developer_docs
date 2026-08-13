---
sidebar_label: "커스텀 -> 페더레이션 계정 전환"
description: "ChangeCustomToFederation"
---

# ChangeCustomToFederation
public BackendReturnObject **ChangeCustomToFederation**(string **federationToken**, FederationType **type**);


## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| federationToken      | string | 페더레이션 access token |
| type      | FederationType(enum) | 페더레이션의 종류.<br />`FederationType.Google`<br />`FederationType.Facebook`<br/>`FederationType.Apple`<br />`FederationType.Steam`<br />`FederationType.GPGS2`<br />`FederationType.Line` |

## 설명
뒤끝 SDK에서 커스텀으로 가입한 아이디를 페더레이션 계정으로 변경하는 기능을 제공합니다.  
커스텀 유저가 로그인을 진행한 후에 ChangeCustomToFederation 함수를 호출하면 페더레이션 계정으로 전환이 가능합니다.  
ChangeCustomToFederation에 필요한 **federationToken과 type**은 [페더레이션 인증 문서](https://docs.thebackend.io/sdk-docs/backend/base/user/federation/signup-and-login)에 더 자세하게 설명되어 있습니다.  
>  커스텀 계정을 페더레이션 계정으로 변경할 때 이메일 권한을 추가하지 않은 경우 회원 아이디는 커스텀 아이디로 표시됩니다.  


## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.ChangeCustomToFederation("federationToken", FederationType.Google);
if(bro.IsSuccess())
{
  Debug.Log("로그인 타입 전환에 성공했습니다");
}
```

### 비동기
```js
Backend.BMember.ChangeCustomToFederation("federationToken", FederationType.Google , callback => {
  if(callback.IsSuccess())
  {
    Debug.Log("로그인 타입 전환에 성공했습니다");
  }
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.ChangeCustomToFederation,  "federationToken", FederationType.Google, callback => {
  if(callback.IsSuccess())
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

**페더레이션 전환에 사용되는 토큰값이 null 로 전송된 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined federation_access_token, federation_access_token을(를) 확인할 수 없습니다

**이미 Federation 계정으로 가입된 계정에 커스텀 아이디 변경을 시도한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated federationId, 중복된 federationId 입니다

