---
sidebar_label: "페더레이션 회원가입/로그인"
---

# AuthorizeFederation

public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**);

## 파라미터

| Value           | Type                 | Description                                         |
| :-------------- | :------------------- | :-------------------------------------------------- |
| federationToken | string               | 페더레이션 token                                    |
| type            | FederationType(enum) | 페더레이션의 종류.<br />`Google, Apple, Facebook, Steam, GPGS2` |

## 설명

구글/애플/페이스북등 연동하려는 플랫폼의 회원정보 토큰 값을 이용하여 회원가입/로그인을 시도합니다.

- 해당 토큰으로 회원가입을 한 내역이 없을 경우 회원가입과 동시에 로그인을 시도합니다.
- 해당 토큰으로 회원가입을 한 내역이 존재할 경우 로그인을 시도합니다.

## Example

### 동기

```js
Backend.MultiCharacter.Account.AuthorizeFederation(
  "federationToken",
  FederationType.Google
);
```

### 비동기

```js
Backend.MultiCharacter.Account.AuthorizeFederation(
  "federationToken",
  FederationType.Google,
  (callback) => {
    // 페더레이션 인증 이후 처리
  }
);
```

### SendQueue

```js
SendQueue.Enqueue(
  Backend.MultiCharacter.Account.AuthorizeFederation,
  "federationToken",
  FederationType.Google,
  (callback) => {
    // 페더레이션 인증 이후 처리
  }
);
```

## ReturnCase

### Success cases

**로그인에 성공한 경우**  
statusCode : 200  
message : Success

**신규 회원가입에 성공한 경우**  
statusCode : 201  
message : Success

### Error cases

**디바이스 정보가 null일 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined device_unique_id, device_unique_id을(를) 확인할 수 없습니다

**차단당한 계정일 경우**  
statusCode : 403  
errorCode : 콘솔에서 입력한 차단된 사유  
message : Forbidden blocked user, 금지된 blocked user

**차단당한 디바이스일 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden blocked device, 금지된 blocked device

**탈퇴가 진행중일 경우(WithdrawAccount 함수 호출 이후)**  
statusCode : 410  
errorCode : GoneResourceException  
message : Gone user, 사라진 user 입니다

> 탈퇴가 완료되면 201로 신규 회원가입이 진행됩니다.

---

# 페더레이션 계정 로그아웃

## 기존에 로그인 된 페더레이션 계정을 로그아웃 후, 다른 페더레이션 유저로 변경하고 싶을 경우

뒤끝 계정을 로그아웃 한 후 페더레이션 계정을 로그아웃해야 정상적으로 다른 페더레이션 계정으로 로그인 할 수 있습니다.  
다른 페더레이션 계정으로 로그인 한 후에는 다시 뒤끝 계정으로 `AuthorizeFederation` 함수를 호출해야 새로운 계정으로 게임을 진행할 수 있습니다.

### GPGS 로그아웃 방법

```js
public void GoogleSignout(){
    GoogleSignIn.DefaultInstance.SignOut();
}
```

### 페이스북 로그아웃 방법

```js
public void FacebookLogOut(){
    FB.LogOut();
}
```
