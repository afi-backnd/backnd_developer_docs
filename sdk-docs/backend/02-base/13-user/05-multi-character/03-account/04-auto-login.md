---
draft: "true"
unlisted: "true"
sidebar_label: "자동 로그인(토큰 로그인)"
description: "AutoLoginAccount"
---

# AutoLoginAccount

public BackendReturnObject **AutoLoginAccount**();


## 설명
이전에 해당 기기에서 로그인을 한 아이디를 통해 자동으로 입력없이 로그인을 시도합니다.  
해당 기기에서 로그인을 한 적이 없는 경우, 앱의 캐시 클리어 혹은 앱을 지웠다 다시 설치한 경우 기기에 저장되어있던 아이디 정보가 삭제되어 자동 로그인이 불가능해집니다.  

## Example

### 동기
```js
BackendReturnObject bro = Backend.MultiCharacter.Account.AutoLoginAccount();
if(bro.IsSuccess()) {
  Debug.Log("계정 로그인에 성공했습니다.");
}
```

### 비동기
```js
BackendReturnObject bro = Backend.MultiCharacter.Account.AutoLoginAccount(callback => {
  if(callback.IsSuccess()) {
  Debug.Log("계정 로그인에 성공했습니다.");
  }
});
```

### SendQueue
```js
SendQueue.Enqueue(BackendReturnObject bro = Backend.MultiCharacter.Account.AutoLoginAccount, callback => {
  if(callback.IsSuccess()) {
    Debug.Log("계정 로그인에 성공했습니다");
  }
});
```

## ReturnCase

### Success cases

**회원가입에 성공한 경우**  
statusCode : 200  
message : Success  

### Error cases

**존재하지 않는 토큰일 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : account access token not found, account access token을(를) 찾을 수 없습니다
