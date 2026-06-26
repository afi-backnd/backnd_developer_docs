---
sidebar_label: 국가코드 등록
---

# UpdateCountryCode
public Task&lt;RequestResult&gt; **UpdateCountryCodeAsync**(CountryCode **code**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| code      | CountryCode | 뒤끝에서 제공하는 국가 코드 |

## 설명
유저의 국가 코드를 등록합니다.  

:::info 정보
CountryCode에 대한 정보는 <a href="/sdk-docs/backend/base/country-code/all-codes" target="_blank">이곳</a>을 참고해 주세요.  
:::

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.UpdateCountryCodeAsync(CountryCode.SouthKorea);
```

### Callback 방식
```js
BackndAuth.Instance.UpdateCountryCodeA(CountryCode.SouthKorea, callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**등록에 성공한 경우**  
statusCode : 204  
message : Success  
