---
sidebar_label: 계정 로그아웃
---

# LogoutAccount
public BackendReturnObject **LogoutAccount**();

## 설명
유저 계정을 로그아웃합니다.  
로그아웃 성공 시, 기기에 저장된 액세스토큰과 리프레시 토큰은 삭제되며, 계정 자동 로그인 기능은 사용이 불가능해집니다.  

## Example

### 동기
```js
BackendReturnObject bro = Backend.MultiCharacter.Account.LogoutAccount();
```

### 비동기
```js
Backend.MultiCharacter.Account.LogoutAccount(callback => {
    // TODO
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.MultiCharacter.Account.LogoutAccount, callback => {
    // TODO
});
```

## ReturnCase

### Success cases
**회원가입에 성공한 경우**  
statusCode : 204  
message : Success  