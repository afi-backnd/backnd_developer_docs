---
sidebar_label: "캐릭터 삭제"
sidebar_position: "4"
description: "DeleteCharacter"
---

# DeleteCharacter

public BackendReturnObject **DeleteCharacter**(string **uuid**, string **inDate**);

## 파라미터

| Value | Type | Description |
| --- | --- | --- |
| uuid | string | 삭제할 캐릭터의 uuid |
| inDate | string | 삭제할 캐릭터의 inDate |

## 설명

`GetCharacterList`에서 받은 `uuid`와 `inDate`에 해당하는 캐릭터를 삭제합니다.

계정 컨텍스트(`Backend.IsMultiAccountLogin == true`) 또는 `SelectCharacter`로 진입한 캐릭터 컨텍스트에서 호출합니다. `Elevate`에 성공하면 계정 컨텍스트로 진입하므로 바로 호출할 수 있습니다.

:::danger 삭제 주의

캐릭터 삭제 시 해당 캐릭터에 귀속된 게임 정보와 닉네임, 리더보드 등 관련 데이터도 삭제됩니다. 복구할 수 없으므로 사용자 확인을 받은 뒤 호출하세요.

:::

## Example

### 동기

```csharp
BackendReturnObject bro = Backend.MultiCharacter.Character.DeleteCharacter(uuid, inDate);

if (bro.IsSuccess())
{
    Debug.Log("캐릭터 삭제 성공");
}
```

### 비동기

```csharp
Backend.MultiCharacter.Character.DeleteCharacter(uuid, inDate, (callback) => {
    if (callback.IsSuccess())
    {
        Debug.Log("캐릭터 삭제 성공");
    }
});
```

### SendQueue

```csharp
SendQueue.Enqueue(Backend.MultiCharacter.Character.DeleteCharacter, uuid, inDate, (callback) => {
    if (callback.IsSuccess())
    {
        Debug.Log("캐릭터 삭제 성공");
    }
});
```

## ReturnCase

### Success cases

**캐릭터 삭제에 성공한 경우**  
statusCode : 204  
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


**멀티 캐릭터를 사용하지 않는 프로젝트인 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : This project does not use multi character. Please change the project to multi character in the BACKND console.

**uuid와 inDate가 일치하는 캐릭터가 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer not found, gamer을(를) 찾을 수 없습니다
