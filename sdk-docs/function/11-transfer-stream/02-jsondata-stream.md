---
sidebar_label: "[Deprecated] JsonData를 Stream으로 변환"
description: "JsonData를 Stream으로 변환"
draft: true
unlisted: true
---

# JsonToStream

```csharp
public static Stream JsonToStream(BackEnd.BackndLitJson.JsonData json);
```

## 파라미터

| Value | Type     | Description                       |
| ----- | -------- | --------------------------------- |
| json  | BackEnd.BackndLitJson.JsonData | stream으로 변환을 원하는 JsonData |

## 설명

JsonData 타입의 데이터를 JSON 문자열로 바꾼 뒤 stream으로 변환합니다.  
`Backend.Content`와 같은 `BackEnd.BackndLitJson.JsonData` 타입만 사용할 수 있습니다.  

## Example

반환할 값을 JsonData로 만들어 변환합니다.  

```csharp
using BackEnd.BackndLitJson;

JsonData result = new JsonData();
result["result"] = "ok";
result["count"] = 3;

return Backend.JsonToStream(result);
```

위 코드는 아래 값을 반환합니다.  

```json
{"result":"ok","count":3}
```

:::caution 뒤끝 기능 호출 결과를 반환하는 경우
뒤끝 기능을 호출한 결과의 `GetReturnValuetoJSON()`은 `LitJson.JsonData` 타입이라 `JsonToStream`에 넣을 수 없습니다.  
결과를 그대로 반환하려면 `GetReturnValue()`로 문자열을 받아 `StringToStream`으로 변환합니다.  

```csharp
var bro = Backend.BMember.GetUserInfo();

return Backend.StringToStream(bro.GetReturnValue());
```
:::

## ReturnCase

stream 이 리턴됩니다.  
