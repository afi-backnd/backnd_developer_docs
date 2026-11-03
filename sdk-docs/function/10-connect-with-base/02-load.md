---
sidebar_label: "[Deprecated] 뒤끝베이스에서 보낸데이터 조회"
description: "뒤끝베이스에서 보낸데이터 조회"
draft: true
unlisted: true
---

# Content

## 설명

SDK에서 송신한 데이터는 `Backend.Content` 변수에 저장되어 있습니다.  
`Backend.Initialize(ref stream)`를 호출한 뒤에 사용할 수 있습니다.  

`Content`의 타입은 `BackEnd.BackndLitJson.JsonData`입니다. `LitJson.dll`의 `LitJson.JsonData`와는 다른 타입이므로 서로 바꾸어 사용할 수 없습니다.  

## Example

```csharp
// SDK에서 key가 mykey이고, value가 hello world인 데이터를 송신한 경우

string data = string.Empty;
if(Backend.HasKey("mykey") == true)
{
    data = Backend.Content["mykey"].ToString();
}
```
