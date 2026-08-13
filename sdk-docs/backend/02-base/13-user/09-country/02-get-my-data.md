---
sidebar_label: "내 국가코드 조회"
description: "GetMyCountryCode"
---

# GetMyCountryCode
public BackendReturnObject **GetMyCountryCode**();

## 설명
자신의 국가 코드를 받아옵니다.  
> 자신의 국가 코드의 경우 [GetUserInfo](/sdk-docs/backend/base/user/get-my-information/get-from-server) 함수를 통해서도 조회할 수 있습니다.  

## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.GetMyCountryCode();
string countryByString = bro.GetReturnValuetoJSON()["country"]["S"].ToString();

//국가코드로 변환
BackEnd.GlobalSupport.CountryCode countryCode = BackEnd.GlobalSupport.CountryCodeDic.GetCountryName(countryByString);

```

### 비동기
```js
Backend.BMember.GetMyCountryCode((callback) =>
{
    string countryBySring  = callback.GetReturnValuetoJSON()["country"]["S"].ToString();

    //국가코드로 변환
    BackEnd.GlobalSupport.CountryCode countryCode = 
    BackEnd.GlobalSupport.CountryCodeDic.GetCountryName(countryBySring);
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.GetMyCountryCode, (callback) =>
{
    string countryByString  = callback.GetReturnValuetoJSON()["country"]["S"].ToString();

    //국가코드로 변환
    BackEnd.GlobalSupport.CountryCode countryCode = 
    BackEnd.GlobalSupport.CountryCodeDic.GetCountryName(countryByString);
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**국가 코드가 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : country not found, country을(를) 찾을 수 없습니다

## GetReturnValuetoJSON
```js
{
    country : {S : "KR"}
}
```
