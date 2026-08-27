---
sidebar_label: "캐릭터 생성"
sidebar_position: "1"
description: "CreateCharacter"
---

# CreateCharacter

public BackendReturnObject **CreateCharacter**(string **nickname**);

## 파라미터

| Value | Type | Description |
| --- | --- | --- |
| nickname | string | 생성할 캐릭터의 닉네임 |

## 설명

멀티 캐릭터 계정에 새로운 캐릭터를 생성합니다.

- 계정 컨텍스트(`Backend.IsMultiAccountLogin == true`) 또는 `SelectCharacter`로 진입한 캐릭터 컨텍스트에서 호출합니다. `Elevate` 직후의 세션은 계정 권한이 없어 호출할 수 없습니다.
- 호출 전에 `Backend.LocationProperties.UpdateLocationProperties` 또는 `CustomizeLocationProperties`로 위치 정보를 불러와야 합니다.
- 닉네임은 중복하여 사용할 수 없습니다.
- 계정 하나에 캐릭터는 최대 20개까지 생성할 수 있습니다.
- 성공 시 `SelectCharacter`에 필요한 `uuid`와 `gamerInDate`를 반환합니다.

## Example

### 동기

```csharp
BackendReturnObject bro = Backend.MultiCharacter.Character.CreateCharacter("Knight");

if (bro.IsSuccess())
{
    LitJson.JsonData value = bro.GetReturnValuetoJSON();
    string uuid = value["uuid"].ToString();
    string inDate = value["gamerInDate"].ToString();
}
```

### 비동기

```csharp
Backend.MultiCharacter.Character.CreateCharacter("Knight", (callback) => {
    if (callback.IsSuccess())
    {
        LitJson.JsonData value = callback.GetReturnValuetoJSON();
        string uuid = value["uuid"].ToString();
        string inDate = value["gamerInDate"].ToString();
    }
});
```

### SendQueue

```csharp
SendQueue.Enqueue(Backend.MultiCharacter.Character.CreateCharacter, "Knight", (callback) => {
    if (callback.IsSuccess())
    {
        LitJson.JsonData value = callback.GetReturnValuetoJSON();
        string uuid = value["uuid"].ToString();
        string inDate = value["gamerInDate"].ToString();
    }
});
```

## ReturnCase

### Success cases

**캐릭터 생성에 성공한 경우**  
statusCode : 201  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**위치 정보를 불러오지 않은 경우**  
statusCode : 400  
errorCode : ValidationException  
message : locationProperties is required for CreateCharacter. Load it first via Backend.LocationProperties.UpdateLocationProperties() or CustomizeLocationProperties().

**멀티 캐릭터 계정으로 전환하지 않은 경우**  
statusCode : 400  
errorCode : UndefinedAccountParameterException  
message : Account information does not exist. The account needs to be elevated. Call Backend.BMember.Elevate to elevate the account.

**캐릭터가 20개를 초과했을 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden character count can not exceed 20, 금지된 character count can not exceed 20

**`Elevate` 직후의 세션에서 호출한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden caller must be account, 금지된 caller must be account

**멀티 캐릭터를 사용하지 않는 프로젝트인 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : This project does not use multi character. Please change the project to multi character in the BACKND console.

**닉네임이 중복되는 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated nickname, 중복된 nickname 입니다

## GetReturnValuetoJSON

```json
{
  "accessToken": null,
  "refreshToken": null,
  "gamerType": "_character_",
  "mode": "multi",
  "gamerInDate": "2026-08-20T03:16:58.652Z",
  "nickname": "Knight",
  "uuid": "9aa72dc0-9c45-11f1-853c-71d1442b5e5b",
  "uid": 1787195818652
}
```

캐릭터 토큰은 생성 시점에 발급되지 않습니다. `accessToken`과 `refreshToken`은 `null`이며, `SelectCharacter`를 호출해야 캐릭터 컨텍스트로 전환됩니다.
