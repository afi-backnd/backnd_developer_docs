---
sidebar_label: "캐릭터 목록 조회"
sidebar_position: "2"
description: "GetCharacterList"
---

# GetCharacterList

public BackendReturnObject **GetCharacterList**();  
public BackendReturnObject **GetCharacterList**(string **tableName**);

## 파라미터

| Value | Type | Description |
| --- | --- | --- |
| tableName | string | (Optional) 캐릭터 정보와 함께 조회할 게임 정보 테이블 이름 |

## 설명

멀티 캐릭터 계정에 속한 캐릭터 목록을 조회합니다.

계정 컨텍스트(`Backend.IsMultiAccountLogin == true`) 또는 `SelectCharacter`로 진입한 캐릭터 컨텍스트에서 호출합니다. `Elevate`에 성공하면 계정 컨텍스트로 진입하므로 바로 호출할 수 있습니다.

`tableName`을 전달하면 각 캐릭터가 해당 테이블에 저장한 최신 row 한 건을 함께 조회합니다. 테이블이 없거나 저장한 row가 없으면 해당 캐릭터 응답에 테이블 이름의 필드가 포함되지 않습니다. 한 번에 테이블 하나만 지정할 수 있습니다. `tableName`을 생략하면 캐릭터 정보만 조회합니다.

## Example

### 동기

```csharp
BackendReturnObject bro = Backend.MultiCharacter.Character.GetCharacterList("UserData");
```

```csharp
// 게임 정보 없이 캐릭터 목록만 조회
BackendReturnObject bro = Backend.MultiCharacter.Character.GetCharacterList();
```

### 비동기

```csharp
Backend.MultiCharacter.Character.GetCharacterList("UserData", (callback) => {
    if (callback.IsSuccess())
    {
        LitJson.JsonData characters = callback.GetReturnValuetoJSON()["characters"];
    }
});
```

### SendQueue

```csharp
SendQueue.Enqueue(Backend.MultiCharacter.Character.GetCharacterList, "UserData", (callback) => {
    if (callback.IsSuccess())
    {
        LitJson.JsonData characters = callback.GetReturnValuetoJSON()["characters"];
    }
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**캐릭터가 존재하지 않는 경우**  
statusCode : 200  
message : Success  
returnValue : `{"characters":[]}`

### Error cases

**멀티 캐릭터 계정으로 전환하지 않은 경우**  
statusCode : 400  
errorCode : UndefinedAccountParameterException  
message : Account information does not exist. The account needs to be elevated. Call Backend.BMember.Elevate to elevate the account.


**멀티 캐릭터를 사용하지 않는 프로젝트인 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : This project does not use multi character. Please change the project to multi character in the BACKND console.

## GetReturnValuetoJSON

```json
{
  "characters": [
    {
      "uuid": "9aa72dc0-9c45-11f1-853c-71d1442b5e5b",
      "nickname": "Knight",
      "inDate": "2026-08-20T03:16:58.652Z",
      "lastLogin": "2026-08-20T03:18:50.961Z",
      "locationProperties": {
        "country": "South Korea",
        "region": "Seoul",
        "city": "Seocho-gu",
        "language": "ko-KR"
      },
      "UserData": {
        "client_date": "2026-08-20T03:17:02.903Z",
        "stage": 3,
        "gold": 1200
      }
    }
  ]
}
```
