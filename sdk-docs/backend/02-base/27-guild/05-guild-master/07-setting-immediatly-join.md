---
sidebar_label: "길드 즉시 가입 설정"
description: "SetRegistrationValueV3"
---

# SetRegistrationValueV3

public BackendReturnObject **SetRegistrationValueV3**(bool **immediateFlag**);

## 파라미터

| Value         | Type | Description                                               |
| ------------- | ---- | --------------------------------------------------------- |
| immediateFlag | bool | 즉시 가입 허용 여부(true - 즉시 가입, false - 승인 가입) |

## 설명

플레이어들이 길드에 가입할 때 즉시 가입을 허용할 것인지를 설정합니다.  
SetRegistrationValueV3를 통해 즉시 가입 여부를 설정하면 길드의 메타 데이터에 \_immediateRegistration 키가 추가됩니다.  
\_immediateRegistration 키는 콘솔의 메타 정보로는 확인되지 않고, 클라이언트를 통해 길드 정보를 조회할때만 확인할 수 있습니다.  
value는 true 혹은 false가 저장됩니다.  

:::danger 주의
* 해당 함수를 통해 즉시 가입 여부를 설정하지 않아도 모든 길드는 길드장 가입 승인 후 가입으로 설정됩니다.  

* 해당 함수를 통해 즉시 가입 여부를 설정하지 않으면 메타 데이터에 _immediateRegistration 키가 추가되지 않습니다.  

* 특정 길드만 즉시 가입 여부를 설정하고, 특정 길드는 즉시 가입 여부를 설정하지 않으면 메타 데이터에 _immediateRegistration 키가 있는 길드와 없는 길드가 혼재될 수 있습니다. 즉시 가입 여부를 설정할 수 있게 개발하려는 개발사는 플레이어가 길드를 생성을 성공한 후 SetRegistrationValueV3 호출을 통해 즉시 가입 여부를 초기화해주는 것을 추천합니다.  
:::

## Example

### 동기

```js
Backend.Guild.SetRegistrationValueV3(true); // 즉시 가입 설정
```

### 비동기

```js
Backend.Guild.SetRegistrationValueV3(true, (callback) => { // 즉시 가입 설정
    // 이후 처리
  });
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.SetRegistrationValueV3,true, (callback) => { // 즉시 가입 설정
    // 이후 처리
  });
```

## ReturnCase

### Success cases

**변경에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**마스터 이외의 길드원이 함수를 호출한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden guild Member, 금지된 guild Member

**길드에 가입하지 않은 플레이어가 함수를 호출한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다


