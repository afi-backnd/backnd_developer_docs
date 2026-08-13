---
sidebar_label: "다른 유저 국가코드 조회"
description: "GetCountryCodeByIndate"
---

# GetCountryCodeByIndate
public BackendReturnObject **GetCountryCodeByIndate**(string **inDate**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| inDate      | string | 유저의 InDate |

## 설명
다른 유저의 국가 코드를 받아옵니다.  

## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.GetCountryCodeByIndate("2020-12-16T04:58:28.604Z");
string countryByString = bro.GetReturnValuetoJSON()["country"]["S"].ToString();

//국가코드로 변환
BackEnd.GlobalSupport.CountryCode countryCode = BackEnd.GlobalSupport.CountryCodeDic.GetCountryName(countryByString);
```

### 비동기
```js
Backend.BMember.GetCountryCodeByIndate("2020-12-16T04:58:28.604Z", (callback) =>
{
    string countryByString = bro.GetReturnValuetoJSON()["country"]["S"].ToString();

    //국가코드로 변환
    BackEnd.GlobalSupport.CountryCode countryCode = 
    BackEnd.GlobalSupport.CountryCodeDic.GetCountryName(countryByString);
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.GetCountryCodeByIndate, "2020-12-16T04:58:28.604Z", (callback) =>
{
    string countryByString = bro.GetReturnValuetoJSON()["country"]["S"].ToString();

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
