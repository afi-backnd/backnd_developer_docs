---
sidebar_label: 뒤끝펑션 리스트 조회
description: "뒤끝펑션 리스트 조회"
---

# GetFunctions

public Task< GetFunctionsResult > **GetFunctionsAsync**();

## 설명

서버에 배포되어 있는 뒤끝펑션들의 리스트를 조회합니다.  

## Example

### Task 방식

```js
var reqRequest = await BackndFunction.Instance.GetFunctionsAsync();
```

### Callback 방식

```js
BackndFunction.Instance.GetFunctions((callback) =>
{
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
