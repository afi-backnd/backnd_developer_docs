---
sidebar_label: 이메일 정보등록
---

# UpdateCustomEmail
public BackendReturnObject **UpdateCustomEmail**(string **emailAddress**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| EmailAddress      | string | 아이디 찾기, 비밀번호 초기화를 진행할 이메일 주소 | 

## 설명
커스텀 계정의 아이디 찾기, 비밀번호 초기화를 위한 이메일 정보를 유저 정보에 등록합니다.  
페더레이션 유저가 등록한 이메일과는 별도로 관리되는 이메일입니다.  

## Example

### 동기
```js
Backend.BMember.UpdateCustomEmail("help@backnd.com");
```

### 비동기
```js
Backend.BMember.UpdateCustomEmail("help@backnd.com", (callback) =>
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.UpdateCustomEmail, "help@backnd.com", (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**수정에 성공한 경우**  
statusCode : 204  
message : Success
