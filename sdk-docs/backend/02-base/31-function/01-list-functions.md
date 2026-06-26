---
sidebar_label: 뒤끝펑션 리스트 조회
---

# GetFunctionList

public BackendReturnObject **GetFunctionList**();

## 설명

서버에 배포되어 있는 뒤끝펑션들의 리스트를 조회합니다.  

## Example

### 동기

```js
Backend.BFunc.GetFunctionList();
```

### 비동기

```js
Backend.BFunc.GetFunctionList((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.BFunc.GetFunctionList, (callback) => {
  // 이후 처리
});
```

## Return Cases

### Success Cases

**조회에 성공한 경우**  
statusCode : 200  
errorCode : Success
returnValue : { functions : \['funcA', 'funcB', ...] }

**조회에 성공하였으나 함수 리스트가 존재하지 않는 경우**  
statusCode : 200  
errorCode : Success
returnValue : { functions : \[] }

### Error Cases

**유니티에서 뒤끝펑션 인증키를 잘못 입력한 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : 'bad BackendFunctionToken, 잘못된 BackendFunctionToken 입니다'
