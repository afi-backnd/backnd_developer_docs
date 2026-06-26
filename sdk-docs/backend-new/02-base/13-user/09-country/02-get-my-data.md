---
sidebar_label: 내 국가코드 조회
---

# GetCountryCode
public Task&lt;GetCountryCodeResult&gt; **GetCountryCodeAsync**();

## 설명
자신의 국가 코드를 받아옵니다.  
> 자신의 국가 코드의 경우 [GetUserInfo](/sdk-docs/backend/base/user/get-my-information/get-from-server) 함수를 통해서도 조회할 수 있습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.GetCountryCodeAsync();
var countryByString = reqResult.GetCountryCode();

//국가코드로 변환
BACKND.Base.GlobalSupport.CountryCode countryCode = BACKND.Base.GlobalSupport.CountryCodeDic.GetCountryName(countryByString);
```

### Callback 방식
```js
BackndAuth.Instance.GetCountryCodeAsync((callback) =>
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
