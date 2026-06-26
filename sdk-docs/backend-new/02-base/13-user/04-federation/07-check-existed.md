---
sidebar_label: 뒤끝 가입 여부 확인(외부 로그인)
---

# CheckLinkedAccount
public Task&lt;CheckLinkedAccountResult&gt; **CheckLinkedAccountAsync**(string **authToken**, AuthProvider **provider**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| authToken     | string | 외부 로그인 인증 token |
| provider      | AuthProvider(enum) | 외부 로그인 제공자의 종류.<br />`AuthProvider.Google`<br />`AuthProvider.Facebook`<br/>`AuthProvider.Apple`<br />`AuthProvider.Steam`<br />`AuthProvider.GPGS2`<br />`AuthProvider.Line` |


## 설명
외부 로그인으로 가입한 유저가 뒤끝 서버에 존재하는지 확인합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.CheckLinkedAccountAsync("authToken", AuthProvider.Google);
```

### Callback 방식
```js
BackndAuth.Instance.CheckLinkedAccountAsync("authToken", AuthProvider.Google, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**가입하지 않은 federationToken인 경우**  
statusCode : 204  
message : Success  

### Error cases

**가입한 federationToken인 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

## ReturnValueJson
해당 게이머의 외부 로그인 연동 관련 정보를 나타냅니다.  
```js
{
    "gamer":
    {
        "os":"android",
        "device":"LGE Nexus 5X",
        "lastLogin":"2018-08-10T01:17:17.859Z",
        "etc":"유저 기타 정보",
        "osVersion":"Android OS 8.0.0 / API-26 &#40OPR4.170623.006/4286358&#41",
        "type":"google"
    }
}
```
