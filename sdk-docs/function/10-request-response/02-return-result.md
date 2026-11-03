---
sidebar_label: 결과 반환하기
description: "결과 반환하기"
---

# 결과 반환하기

`Function` 함수가 반환한 `Stream`이 함수를 호출한 유니티로 전달됩니다.  
문자열은 `Backend.StringToStream`, JSON은 `Backend.JsonToStream`으로 `Stream`을 만들어 반환합니다.  

## 유니티가 받는 값

함수가 반환한 값은 `InvokeFunction` 결과의 `ReturnValue`에서 `result` 아래에 들어갑니다.  

| 함수가 반환한 값 | 유니티의 `ReturnValue` |
| --- | --- |
| `Backend.StringToStream("HelloWorld!")` | `{"result":"HelloWorld!"}` |
| `{"ok":true,"gold":100}`을 반환 | `{"result":{"ok":true,"gold":100}}` |

```csharp
var bro = Backend.BFunc.InvokeFunction("market", param);
if (bro.IsSuccess() == false)
{
    Debug.LogError("뒤끝펑션 호출 실패: " + bro);
    return;
}

var result = bro.GetReturnValuetoJSON()["result"];
```

:::caution 호출 성공과 함수의 성공은 다릅니다
함수 안에서 오류가 나도 `InvokeFunction`은 성공(`IsSuccess() == true`)으로 끝납니다.  
함수가 의도한 동작을 했는지는 `result`의 내용으로 판단합니다. 아래 [오류 돌려주기](#오류-돌려주기)처럼 함수가 성공과 실패를 결과에 담아 주세요.  
:::

## 문자열 반환하기

```csharp
public static Stream StringToStream(string text);
```

| 파라미터 | 타입 | 설명 |
| --- | --- | --- |
| text | string | 반환할 문자열 |

```csharp
return Backend.StringToStream("success");
```

## JSON 반환하기

```csharp
public static Stream JsonToStream(BackEnd.BackndLitJson.JsonData json);
```

| 파라미터 | 타입 | 설명 |
| --- | --- | --- |
| json | BackEnd.BackndLitJson.JsonData | 반환할 JSON |

JSON을 JSON 문자열로 바꾼 뒤 `Stream`으로 변환합니다.  
`Backend.Content`와 같은 `BackEnd.BackndLitJson.JsonData` 타입만 사용할 수 있습니다.  

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

`Newtonsoft.Json`의 `JObject`로 만든 JSON은 문자열로 바꿔 `StringToStream`으로 반환합니다.  

```csharp
JObject result = new JObject();
result.Add("ok", true);

return Backend.StringToStream(result.ToString());
```

:::caution JSON 안에 JSON 문자열을 넣지 마세요
`{"user":"{\"level\":3}"}`처럼 문자열 값 안에 JSON 문자열을 넣어 반환하면, 서버가 결과를 전달하면서 안쪽 따옴표의 이스케이프를 풀어 유효하지 않은 JSON이 될 수 있습니다.  
`{"user":{"level":3}}`처럼 JSON 객체로 넣어 반환해 주세요.  
:::

:::caution 뒤끝 기능 호출 결과를 반환하는 경우
뒤끝 기능 호출 결과의 `GetReturnValuetoJSON()`은 `LitJson.JsonData` 타입이라 `JsonToStream`에 넣을 수 없습니다.  
결과를 그대로 반환하려면 `GetReturnValue()`로 문자열을 받아 `StringToStream`으로 변환합니다.  

```csharp
var bro = Backend.BMember.GetUserInfo();

return Backend.StringToStream(bro.GetReturnValue());
```
:::

## 오류 돌려주기

함수가 처리를 끝내지 못했을 때는 예외를 밖으로 던지지 말고, 원인을 담은 JSON을 반환합니다.  
템플릿과 튜토리얼은 아래 함수를 사용합니다.  

```csharp
static Stream ReturnErrorObject(string err)
{
    JObject error = new JObject();
    error.Add("error", err);

    return Backend.StringToStream(error.ToString());
}
```

유니티에서는 `result`에 `error`가 있는지로 실패를 구분합니다.  

```csharp
var result = bro.GetReturnValuetoJSON()["result"];
if (result.IsObject && result.ContainsKey("error"))
{
    Debug.LogError("함수 처리 실패: " + result["error"]);
}
```
