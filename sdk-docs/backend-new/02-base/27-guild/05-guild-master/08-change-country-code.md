---
sidebar_label: 국가코드 수정
---

# UpdateCountryCode

public Task< RequestResult > **UpdateCountryCodeAsync**(CountryCode **code**);

## 파라미터

| Value | Type        | Description                 |
| ----- | ----------- | --------------------------- |
| code  | CountryCode | 뒤끝에서 제공하는 국가 코드 |

## 설명

길드의 국가 코드를 수정합니다.  

- 길드 v3 에서만 국가 코드를 수정할 수 있습니다.  
- 길드 마스터만 국가 코드를 수정할 수 있습니다.  
- 길드의 국가 코드 정보는 GetUserGuildInfo의 결괏값에 **countryCode** 키값으로 조회할 수 있습니다.  

## Example

### Task 방식

```js
// 국가 코드 한국으로 설정
var reqResult = await BackndGuild.Instance.UpdateCountryCodeAsync(CountryCode.SouthKorea);
```

### Callback 방식

```js
// 국가 코드 한국으로 설정
BackndGuild.Instance.UpdateCountryCode(CountryCode.SouthKorea, (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**변경에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**길드 마스터 외의 유저가 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden you are not guild master, 금지된 you are not guild master
