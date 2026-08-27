---
sidebar_label: "캐릭터 선택"
sidebar_position: "3"
description: "SelectCharacter"
---

# SelectCharacter

public BackendReturnObject **SelectCharacter**(string **uuid**, string **inDate**);

## 파라미터

| Value | Type | Description |
| --- | --- | --- |
| uuid | string | 선택할 캐릭터의 uuid |
| inDate | string | 선택할 캐릭터의 inDate |

## 설명

`GetCharacterList`에서 받은 `uuid`와 `inDate`에 해당하는 캐릭터를 선택합니다. 성공하면 캐릭터 컨텍스트로 전환되며 게임 정보, 리더보드, 우편 등 뒤끝 베이스 기능은 선택한 캐릭터를 기준으로 동작합니다.

캐릭터 컨텍스트에서도 계정 권한은 유지되므로, 로그아웃하지 않고 다시 호출해 다른 캐릭터로 전환할 수 있습니다. 단 `Elevate` 직후의 세션은 계정 권한이 없어 호출할 수 없습니다.

## Example

### 동기

```csharp
BackendReturnObject bro = Backend.MultiCharacter.Character.SelectCharacter(uuid, inDate);

if (bro.IsSuccess() && Backend.IsMultiCharacterLogin)
{
    Debug.Log("캐릭터 선택 성공");
}
```

### 비동기

```csharp
Backend.MultiCharacter.Character.SelectCharacter(uuid, inDate, (callback) => {
    if (callback.IsSuccess() && Backend.IsMultiCharacterLogin)
    {
        Debug.Log("캐릭터 선택 성공");
    }
});
```

### SendQueue

```csharp
SendQueue.Enqueue(Backend.MultiCharacter.Character.SelectCharacter, uuid, inDate, (callback) => {
    if (callback.IsSuccess() && Backend.IsMultiCharacterLogin)
    {
        Debug.Log("캐릭터 선택 성공");
    }
});
```

## ReturnCase

### Success cases

**캐릭터 선택에 성공한 경우**  
statusCode : 200  
message : Success

### Error cases

**uuid 또는 inDate가 비어 있는 경우**  
statusCode : 400  
errorCode : ValidationException  
message : uuid is null or empty 또는 inDate is null or empty

**멀티 캐릭터 계정으로 전환하지 않은 경우**  
statusCode : 400  
errorCode : UndefinedAccountParameterException  
message : Account information does not exist. The account needs to be elevated. Call Backend.BMember.Elevate to elevate the account.

**`Elevate` 직후의 세션에서 호출한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden caller must be account, 금지된 caller must be account

**멀티 캐릭터를 사용하지 않는 프로젝트인 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : This project does not use multi character. Please change the project to multi character in the BACKND console.

**uuid와 inDate가 일치하는 캐릭터가 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer not found, gamer을(를) 찾을 수 없습니다
