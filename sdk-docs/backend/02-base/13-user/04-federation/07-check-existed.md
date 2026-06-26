---
sidebar_label: 뒤끝 가입 여부 확인(페더레이션)
---

# CheckUserInBackend
public BackendReturnObject **CheckUserInBackend**(string **federationToken**, FederationType **type**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| federationToken      | string | 페더레이션 token |
| type      | FederationType(enum) | 페더레이션의 종류.<br />`FederationType.Google`<br />`FederationType.Facebook`<br/>`FederationType.Apple`<br />`FederationType.Steam`<br />`FederationType.GPGS2`<br />`FederationType.Line` |



## 설명
페더레이션 유저가 게이머로서 뒤끝 서버에 존재하는지 확인합니다.  

## Example

### 동기
```js
Backend.BMember.CheckUserInBackend("federationToken", FederationType.Google);
```

### 비동기
```js
Backend.BMember.CheckUserInBackend("federationToken", FederationType.Google, (callback) =>
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.CheckUserInBackend, "federationToken", FederationType.Google, (callback) =>
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
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON
해당 게이머의 페더레이션 로그인 관련 정보를 나타냅니다.  
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
