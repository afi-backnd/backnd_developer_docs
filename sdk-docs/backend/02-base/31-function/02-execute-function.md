---
sidebar_label: "뒤끝펑션 실행"
description: "InvokeFunction"
---

# InvokeFunction

public BackendReturnObject **InvokeFunction**(string **funcName**, Param **body**);

## 파라미터

| Value    | Type   | Description                    |
| :------- | :----- | :----------------------------- |
| funcName | string | 호출하고자 하는 함수 명        |
| body     | Param  | 함수의 인자 값으로 넘길 데이터 |

## 설명

서버에 배포되어 있는 뒤끝펑션을 실행합니다.  

## Example

### 동기

```js
Param param = new Param();
param.Add("value", "HelloWorld");

Backend.BFunc.InvokeFunction("funcName", param);
```

### 비동기

```js
Param param = new Param();
param.Add("value", "HelloWorld");

Backend.BFunc.InvokeFunction("funcName", param, (callback) =>
{
    // 이후 처리
});
```

### SendQueue

```js
Param param = new Param();
param.Add("value", "HelloWorld");

SendQueue.Enqueue(Backend.BFunc.InvokeFunction, "funcName", param, (callback) =>
{
    // 이후 처리
});
```

## Return Cases

### Success Cases

**함수 호출에 성공한 경우**  
statusCode : 200  
errorCode : Success
returnValue : { result: {''} }

**함수 호출에는 성공했으나 뒤끝펑션 내부에서 에러가 발생한 경우**  

> **뒤끝펑션 내부에서 에러가 발생한 경우에도 성공으로 리턴**됩니다.  
> 뒤끝펑션 내부에서의 성공/실패 여부는 개발사에서 returnValue를 재가공하여 별도 처리해야 합니다.  

statusCode : 200  
errorCode : Success
returnValue : { result: {''} }

### Error Cases

**함수명에 한글이 포함된 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad functionName, 잘못된 functionName 입니다

**유니티에서 뒤끝펑션 인증키를 잘못 입력한 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : 'bad BackendFunctionToken, 잘못된 BackendFunctionToken 입니다'

**존재하지 않는 함수명을 호출한 경우**  
statusCode : 404  
errorCode : ResourceNotFoundException  
message : Function not found: "호출한 함수 명"
