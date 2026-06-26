---
sidebar_label: 이메일 정보 등록
description: "이메일 정보 등록"
---

# UpdateProviderEmail
public Task&lt;RequestResult&gt; **UpdateProviderEmailAsync**(string **authToken**, AuthProvider **provider**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| authToken     | string | 외부 로그인 인증 token |
| provider      | AuthProvider(enum) | 외부 로그인 제공자의 종류.<br />`AuthProvider.Google`<br />`AuthProvider.Facebook`<br/>`AuthProvider.Apple` |

## 설명
기존 외부 로그인으로 가입한 회원들 중 email 수집 안된 회원들의 이메일 정보를 저장합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.UpdateProviderEmailAsync("authToken", AuthProvider.Google);
```

### Callback 방식
```js
BackndAuth.Instance.UpdateProviderEmailAsync("authToken", AuthProvider.Google, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**이메일 정보 업데이트에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**유저 정보가 뒤끝 데이터베이스에 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : federationId not found, federationId을(를) 찾을 수 없습니다
