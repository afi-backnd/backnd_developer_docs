---
sidebar_label: 계정 로그아웃
---

# LogoutAccount
public Task&lt;RequestResult&gt; **LogoutAccountAsync**();

## 설명
유저 계정을 로그아웃합니다.  
로그아웃 성공 시, 기기에 저장된 액세스토큰과 리프레시 토큰은 삭제되며, 계정 자동 로그인 기능은 사용이 불가능해집니다.  

## Example

### Task 방식
```js
var reqResult = await BackndMultiCharacter.Account.LogoutAccountAsync();
```

### Callback 방식
```js
BackndMultiCharacter.Account.LogoutAccount((callback) =>
{
    // ToDo
});
```

## ReturnCase

### Success cases
**회원가입에 성공한 경우**  
statusCode : 204  
message : Success  
