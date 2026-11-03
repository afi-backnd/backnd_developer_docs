---
sidebar_label: 요청 값 읽기
description: "요청 값 읽기"
---

# 요청 값 읽기

유니티에서 `Backend.BFunc.InvokeFunction`으로 보낸 `Param`은 함수 안에서 `Backend.Content`로 읽습니다.  
내 PC에서 `backend debug`로 실행할 때는 프로젝트의 `requests` 폴더에 있는 요청 파일의 내용이 `Param` 대신 들어옵니다. 요청 파일은 [로컬 실행](/sdk-docs/function/cli/debug#요청-파일) 문서를 참고해 주세요.  

## 1. SDK 초기화하기

`Function` 함수가 시작할 때 `Backend.Initialize(ref stream)`를 한 번 호출합니다.  
호출한 뒤부터 `Backend.Content`와 `Backend.HasKey`를 사용할 수 있습니다. 호출한 뒤에는 `stream`을 직접 읽지 않습니다.  

```csharp
public Stream Function(Stream stream, ILambdaContext context)
{
    try
    {
        Backend.Initialize(ref stream);
    }
    catch (Exception e)
    {
        return ReturnErrorObject("initialize " + e.Message);
    }

    // 여기부터 Backend.Content를 사용할 수 있습니다.
}
```

`ReturnErrorObject`는 오류를 JSON으로 돌려주는 함수입니다. [결과 반환하기](/sdk-docs/function/request-response/return-result#오류-돌려주기) 문서를 참고해 주세요.  

## 2. 값이 있는지 확인하기

```csharp
public static bool HasKey(string key);
```

| 파라미터 | 타입 | 설명 |
| --- | --- | --- |
| key | string | 확인할 값의 키 |

요청에 `key`가 있으면 `true`, 없으면 `false`를 반환합니다.  
유니티에서 키를 빠뜨리거나 오타를 내도 함수는 실행되므로, 값을 읽기 전에 먼저 확인합니다.  

```csharp
if (Backend.HasKey("value") == false)
{
    return ReturnErrorObject("value key is not exist");
}
```

## 3. 값 읽기

`Backend.Content`의 타입은 `BackEnd.BackndLitJson.JsonData`입니다.  
`LitJson.dll`의 `LitJson.JsonData`와는 다른 타입이므로 서로 바꾸어 사용할 수 없습니다.  

```csharp
// 문자열
string itemId = Backend.Content["itemId"].ToString();

// 숫자: 문자열로 바꾼 뒤 변환합니다. 변환에 실패하면 오류를 돌려줍니다.
if (long.TryParse(Backend.Content["price"].ToString(), out long price) == false)
{
    return ReturnErrorObject("price must be a number");
}
```

유니티에서 아래와 같이 보내면 위 코드의 `itemId`는 `"sword-0001"`, `price`는 `500`이 됩니다.  

```csharp
Param param = new Param();
param.Add("itemId", "sword-0001");
param.Add("price", 500);

Backend.BFunc.InvokeFunction("market", param);
```

## 요청 값은 그대로 믿지 않습니다

요청 값은 게임 클라이언트가 정합니다. 변조된 클라이언트는 어떤 값이든 보낼 수 있습니다.  

- 가격, 보상 수량, 확률처럼 조작되면 안 되는 값은 요청 값으로 정하지 않고 함수 안이나 서버 데이터에서 정합니다.  
- 숫자는 범위(0보다 큰지, 상한을 넘지 않는지)를 확인한 뒤 사용합니다.  
- **누가 호출했는지는 요청 값으로 판단하지 않습니다.** `userId` 같은 값을 받아 그 사용자의 데이터를 바꾸면, 다른 사람의 데이터를 바꿀 수 있게 됩니다. 호출한 사용자는 [호출한 사용자 확인하기](/sdk-docs/function/request-response/caller) 문서의 방법으로 확인합니다.  
