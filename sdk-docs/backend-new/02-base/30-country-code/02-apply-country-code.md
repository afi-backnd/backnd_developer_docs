---
sidebar_label: 국가코드를 CountryCode로 바꾸기
description: "국가코드를 CountryCode로 바꾸기"
---

# GetCountryName

public CountryCode **GetCountryName**(string **Code**);

## 파라미터

| Value | Type   | Description                                          |
| ----- | ------ | ---------------------------------------------------- |
| code  | string | 서버에서 클라이언트로 송신하는 유저/길드의 국가 코드 |

## 설명

서버에 저장되는 ISO 3166-1 alpha-2 형태의 두 개의 알파벳으로 이루어진 국가 코드를 SDK에서 사용하기 위한 CountryCode로 변환합니다.  

- ISO 3166-1 alpha-2 형태의 국가 코드 및 CountryCode는 [CountryCode 문서](/sdk-docs/backend/base/country-code/all-codes)에서 확인하실 수 있습니다.  

CountryCode를 사용하기 위해서는 아래 구문을 스크립트 상단에 추가합니다.  

```csharp
using BACKND.Base.GlobalSupport;
```

## Example

```js
CountryCodeDic.GetCountryName("KR");

CountryCode country = CountryCodeDic.GetCountryName("KR");
var reqRequest = await BackndAuth.Instance.UpdateCountryCodeAsync(country);

```

## ReturnCase

### Success cases

**변경에 성공한 경우**  
return : **CountryCode.국가코드**  
(ex : KR > CountryCode.SouthKorea)

### Error cases

**존재하지 않는 국가 코드를 입력한 경우**  
return : CountryCode.NONE
