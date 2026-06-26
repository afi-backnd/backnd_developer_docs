---
sidebar_label: 다른 유저 국가코드 조회
---

# GetCountryCodeByInDate
public Task&lt;GetCountryCodeResult&gt; **GetCountryCodeByInDateAsync**(string **inDate**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| inDate      | string | 유저의 InDate |

## 설명
다른 유저의 국가 코드를 받아옵니다.  

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.GetCountryCodeByInDateAsync("2020-12-16T04:58:28.604Z");
var countryByString = reqResult.GetCountryCode();

//국가코드로 변환
BACKND.Base.GlobalSupport.CountryCode countryCode = BACKND.Base.GlobalSupport.CountryCodeDic.GetCountryName(countryByString);
```

### Callback 방식
```js
BackndAuth.Instance.GetCountryCodeByInDate("2020-12-16T04:58:28.604Z", (callback) =>
{
    var countryBySring  = callback.GetCountryCode();

    //국가코드로 변환
    BACKND.Base.GlobalSupport.CountryCode countryCode = 
    BACKND.Base.GlobalSupport.CountryCodeDic.GetCountryName(countryBySring);
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

### Error cases

**국가 코드가 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : country not found, country을(를) 찾을 수 없습니다

## ReturnValueJson
```js
{
    country : {S : "KR"}
}
```
