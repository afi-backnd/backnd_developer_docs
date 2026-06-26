---
sidebar_label: 닉네임 수정
---

# UpdateNickname
public BackendReturnObject **UpdateNickname**(string **nickname**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| nickname    | string      |  닉네임 값. 최대 20자. 앞뒤 공백 비허용. |

## 설명
닉네임을 수정하는 기능입니다.  
> 닉네임이 없는 유저가 닉네임 수정 함수를 호출하는 경우 닉네임 생성을 수행합니다.  

## Example

### 동기
```js
Backend.BMember.UpdateNickname("thebackend");
```

### 비동기
```js
Backend.BMember.UpdateNickname("thebackend", (callback) =>
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.UpdateNickname,"thebackend", (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**생성/수정에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**빈 닉네임 혹은 string.empty로 닉네임 생성&수정을 시도 한 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined nickname, nickname을(를) 확인할 수 없습니다

**20자 이상의 닉네임인 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad nickname is too long, 잘못된 nickname is too long 입니다

**닉네임에 앞/뒤 공백이 있는 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad beginning or end of the nickname must not be blank , 잘못된 beginning or end of the nickname must not be blank 입니다

**이미 중복된 닉네임이 있는 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated nickname, 중복된 nickname 입니다

