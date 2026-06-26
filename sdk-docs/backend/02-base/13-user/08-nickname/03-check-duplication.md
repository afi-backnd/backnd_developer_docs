---
sidebar_label: 닉네임 중복 확인
---

# CheckNicknameDuplication
public BackendReturnObject **CheckNicknameDuplication**(string **nickname**);

## 파라미터

| value  |  description |
| --- | --- |
| nickname | 중복체크할 닉네임 값. 최대 20자. 앞뒤 공백 비허용. |

## 설명
다른 유저가 해당 닉네임을 사용하고 있는지 확인합니다.  


## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.CheckNicknameDuplication("thebackend");
if(bro.IsSuccess())
{
Debug.Log("해당 닉네임으로 수정 가능합니다");
}
```

### 비동기
```js
Backend.BMember.CheckNicknameDuplication("thebackend", (callback) =>
{
   Debug.Log("해당 닉네임으로 수정 가능합니다");
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.CheckNicknameDuplication, "thebackend", (callback) =>
{
    Debug.Log("해당 닉네임으로 수정 가능합니다");
});
```

## ReturnCase

### Success cases

**해당 닉네임이 없는 경우**  
statusCode : 204  
message : Success  

### Error cases

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

