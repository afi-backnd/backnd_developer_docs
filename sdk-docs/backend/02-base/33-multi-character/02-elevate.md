---
sidebar_label: "계정 전환"
sidebar_position: "2"
description: "Elevate"
---

# Elevate

public BackendReturnObject **Elevate**();

## 설명

로그인한 기존 계정을 멀티 캐릭터 계정으로 전환합니다.

- 로그인 후 `Backend.NeedsElevation`이 `true`일 때 호출합니다. 이 값은 멀티 캐릭터를 사용하지 않는 프로젝트에서도 항상 `true`이므로, 멀티 캐릭터 프로젝트에서만 승격 분기를 작성해 주세요.
- 전환에 성공하면 전환 전의 게이머가 계정의 첫 번째 캐릭터로 유지되고, 계정 토큰이 발급됩니다.
- 전환 직후 `Backend.IsMultiAccountLogin`이 `true`인 계정 컨텍스트가 되므로 로그아웃 없이 캐릭터를 조회·생성·선택·삭제할 수 있습니다. 뒤끝 베이스 기능을 사용하려면 `GetCharacterList`로 첫 번째 캐릭터를 조회한 후 `SelectCharacter`를 호출해 캐릭터 컨텍스트로 전환하세요.
- 멀티 캐릭터를 사용하지 않는 프로젝트에서는 전환할 수 없습니다.

## Example

### 동기

```csharp
if (Backend.NeedsElevation)
{
    BackendReturnObject bro = Backend.BMember.Elevate();

    if (!bro.IsSuccess() || !Backend.IsMultiAccountLogin)
    {
        Debug.LogError(bro.ToString());
        return;
    }
}
```

### 비동기

```csharp
Backend.BMember.Elevate((callback) => {
    if (!callback.IsSuccess() || !Backend.IsMultiAccountLogin)
    {
        Debug.LogError(callback.ToString());
        return;
    }
});
```

### SendQueue

```csharp
SendQueue.Enqueue(Backend.BMember.Elevate, (callback) => {
    if (!callback.IsSuccess() || !Backend.IsMultiAccountLogin)
    {
        Debug.LogError(callback.ToString());
        return;
    }
});
```

## ReturnCase

### Success cases

**계정 전환에 성공한 경우**  
statusCode : 200  
message : Success

> Elevate는 전환 전의 게이머를 계정의 첫 번째 캐릭터로 유지하고 계정 토큰을 발급합니다. 성공하면 `Backend.NeedsElevation`이 `false`, `Backend.IsMultiAccountLogin`이 `true`로 변경됩니다.
>
> `GetCharacterList`를 호출하면 전환 전의 닉네임과 inDate를 그대로 가진 첫 번째 캐릭터가 조회됩니다. 해당 캐릭터로 뒤끝 베이스 기능을 사용하려면 `SelectCharacter`를 호출하세요.

### Error cases

**로그인하지 않고 호출한 경우**  
statusCode : 400  
errorCode : BadRequestException  
message : Login is yet

**멀티 캐릭터를 사용하지 않는 프로젝트인 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden elevation not available for single-mode project, 금지된 elevation not available for single-mode project

**캐릭터를 선택한 상태에서 호출한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden UnknownError, 금지된 UnknownError

**이미 멀티 캐릭터 계정으로 로그인한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : \_character\_ gamer not found, \_character\_ gamer을(를) 찾을 수 없습니다
