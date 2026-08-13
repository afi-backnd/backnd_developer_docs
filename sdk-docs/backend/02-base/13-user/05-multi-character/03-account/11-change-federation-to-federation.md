---
sidebar_label: "[New] 페더레이션 계정으로 전환"
description: "ChangeToFederation"
---

# ChangeToFederation
public BackendReturnObject **ChangeToFederation**(string **federationToken**, FederationType **type**);  

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| federationToken      | string | 페더레이션 token |
| type      | FederationType(enum) | 페더레이션의 종류.<br />`FederationType.Google`<br />`FederationType.Facebook`<br/>`FederationType.Apple`<br />`FederationType.Steam`<br />`FederationType.GPGS2` |

## 설명
커스텀 또는 페더레이션 계정으로 로그인되어 있는 계정을 다른 페더레이션 계정으로 전환하는 기능을 제공합니다.  
유저가 로그인 된 상태에서 ChangeToFederation 함수를 호출하면 기존 계정 정보를 유지하면서 현재 계정을 다른 페더레이션 계정으로 전환할 수 있습니다.  
가입되어 있지 않은 상태의 계정으로만 페더레이션 전환이 가능합니다.  
전환 후에는 새로 연결된 페더레이션 계정으로 로그인하여 기존 게임 데이터를 사용할 수 있습니다.  
기존에 사용하던 커스텀 계정이나 페더레이션 계정으로 로그인 하시면 새로운 계정이 생성됩니다.  
ChangeToFederation 함수에 필요한 **federationToken과 type** 정보는 [페더레이션 인증 문서](https://docs.thebackend.io/sdk-docs/backend/base/user/multi-character/account/signup-and-login)에서 자세히 확인할 수 있습니다.  

## Example

### 동기
```js
Backend.MultiCharacter.Account.ChangeToFederation("federationToken" , FederationType.Google);
```

### 비동기
```js
Backend.MultiCharacter.Account.ChangeToFederation("federationToken" , FederationType.Google, callback =>
{
    // 페더레이션 인증 이후 처리
});
```
### SendQueue
```js
SendQueue.Enqueue(Backend.MultiCharacter.Account.ChangeToFederation, "federationToken" , FederationType.Google, callback =>
{
    // 페더레이션 인증 이후 처리
});
```

## ReturnCase

### Success cases

**전환에 성공한 경우**  
statusCode : 200  
message : Success  
