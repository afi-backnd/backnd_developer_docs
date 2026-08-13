---
sidebar_label: "[New] 페더레이션 -> 페더레이션 계정 전환"
description: "ChangeToFederation"
---

# ChangeToFederation
public BackendReturnObject **ChangeToFederation**(string **federationToken**, FederationType **type**);


## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| federationToken      | string | 페더레이션 access token |
| type      | FederationType(enum) | 페더레이션의 종류.<br />`FederationType.Google`<br />`FederationType.Facebook`<br/>`FederationType.Apple`<br />`FederationType.Steam`<br />`FederationType.GPGS2`<br />`FederationType.Line` |

## 설명
뒤끝 SDK에서 페더레이션으로 가입한 계정을 다른 페더레이션 계정으로 변경하는 기능을 제공합니다.  
페더레이션으로 가입한 유저가 로그인을 진행한 후에 ChangeToFederation 함수를 호출하면 다른 페더레이션 계정으로 전환이 가능합니다.  
가입되어 있지 않은 상태의 계정으로만 페더레이션 전환이 가능합니다.  
전환 후에는 새로 연결된 페더레이션 계정으로 로그인하여 기존 게임 데이터를 사용할 수 있습니다.  
기존에 사용하던 페더레이션 계정으로 로그인 하시면 새로운 계정이 생성됩니다.  
ChangeToFederation함수에 필요한 **federationToken과 type**은 [페더레이션 인증 문서](https://docs.thebackend.io/sdk-docs/backend/base/user/federation/signup-and-login)에 더 자세하게 설명되어 있습니다.  


## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.ChangeToFederation("federationToken", FederationType.Google);
if(bro.IsSuccess())
{
  Debug.Log("로그인 타입 전환에 성공했습니다");
}
```

### 비동기
```js
Backend.BMember.ChangeToFederation("federationToken", FederationType.Google , callback => {
  if(callback.IsSuccess())
  {
    Debug.Log("로그인 타입 전환에 성공했습니다");
  }
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.ChangeToFederation,  "federationToken", FederationType.Google, callback => {
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

