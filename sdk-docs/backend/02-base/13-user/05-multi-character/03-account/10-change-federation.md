---
sidebar_label: "[Deprecated] 페더레이션 계정으로 전환"
---

# ChangeCustomToFederation
public BackendReturnObject **ChangeCustomToFederation**(string **federationToken**, FederationType **type**);  

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| federationToken      | string | 페더레이션 token |
| type      | FederationType(enum) | 페더레이션의 종류.<br />`FederationType.Google`<br />`FederationType.Facebook`<br/>`FederationType.Apple`<br />`FederationType.Steam`<br />`FederationType.GPGS2` |

## 설명
커스텀 계정으로 로그인되어 있는 계정을 구글/애플/페이스북등 연동하려는 플랫폼의 회원정보를 이용한 계정으로 전환합니다.  
이후 로그인 시, 기존에 이용하던 커스텀 계정으로는 로그인이 불가능하며, 페데레이션 로그인을 이용하셔야 합니다.

## Example

### 동기
```js
Backend.MultiCharacter.Account.ChangeCustomToFederation("federationToken" , FederationType.Google);
```

### 비동기
```js
Backend.MultiCharacter.Account.ChangeCustomToFederation("federationToken" , FederationType.Google, callback =>
{
    // 페더레이션 인증 이후 처리
});
```
### SendQueue
```js
SendQueue.Enqueue(Backend.MultiCharacter.Account.ChangeCustomToFederation, "federationToken" , FederationType.Google, callback =>
{
    // 페더레이션 인증 이후 처리
});
```

## ReturnCase

### Success cases

**전환에 성공한 경우**  
statusCode : 204  
message : Success  
