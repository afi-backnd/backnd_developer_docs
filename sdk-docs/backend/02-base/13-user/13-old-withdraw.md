---
sidebar_label: "[Deprecated] 회원 탈퇴"
draft: "true"
unlisted: "true"
description: "SignOut"
---

# SignOut

public BackendReturnObject **SignOut**();  
public BackendReturnObject **SignOut**(string **reason**);

## 파라미터

| Value  | Type   | Description |
| :----- | :----- | :---------- |
| reason | string | 탈퇴 이유   |

## 설명

서버에서 회원 탈퇴를 진행합니다.  
뒤끝이 제공하는 회원 탈퇴는 7일의 유예기간이 존재합니다.  
이 기간에 다시 같은 아이디로 로그인을 시도하면 자동으로 탈퇴 철회가 되며 다시 사용할 수 있습니다.  
즉시 탈퇴를 원할 경우 뒤끝 콘솔에서 해당 유저를 즉시 탈퇴 처리 시킬 수 있습니다.  

> 회원 탈퇴 함수를 호출할 경우, 기기에 저장된 액세스토큰과 리프레시토큰이 제거가됩니다.  
> 그러나 회원 탈퇴를 진행한 후에 게임을 종료하지 않고 토큰으로 자동로그인(Backend.BMember.LoginWithTheBackendToken())을 실행할 경우, 이미 불러온 로그인 데이터를 이용하기에 로그인이 가능하며 회원 탈퇴가 취소됩니다.  
> 재로그인을 원하지 않을 경우에는 게임을 종료하고 재실행시켜주시기 바랍니다.  

## Example

### 동기

```js
Backend.BMember.SignOut("재미가 없습니다");
```

### 비동기

```js
Backend.BMember.SignOut("재미가 없습니다", (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.BMember.SignOut, "재미가 없습니다", (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**회원 탈퇴에 성공한 경우**  
statusCode : 204  
message : Success  
