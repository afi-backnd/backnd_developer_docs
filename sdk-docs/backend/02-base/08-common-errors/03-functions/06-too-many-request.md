---
sidebar_label: 과도한 서버 요청
sidebar_position: 6
---

# IsTooManyRequestError

public bool **IsTooManyRequestError**();

## 설명

단기간에 **한 ip에서** 많은 요청을 보낼 경우 발생하는 403 Forbbiden 에러 발생 시, true가 리턴됩니다.  
많은 요청의 기준은 **1초에 1번 이상의 요청을 5분동안 지속적으로 보낼 경우** 입니다.  

포함되는 에러케이스는 다음과 같습니다.  

- **과도한 요청이 감지된 경우**  
  statusCode : 403  
  errorCode : Forbidden  
  message : 403 Forbidden(아래와 같은 에러코드)
  > 예외 처리 제공 함수 : [IsTooManyRequestError](/sdk-docs/backend/base/common-errors/functions/too-many-request)

```js
<html>
  <head>
    <title>403 Forbidden</title>
  </head>
  <body>
    <center>
      <h1>403 Forbidden</h1>
    </center>
  </body>
</html>
```

  


- **과도한 요청 감지 이후 금지 시간동안 요청을 보낼 경우**  
  statusCode : 403  
  errorCode : Forbidden  
  message : 403 Forbidden by Local
  > 예외 처리 제공 함수 : [IsTooManyRequestError](/sdk-docs/backend/base/common-errors/functions/too-many-request)

## Example

```js
BackendReturnObject bro = Backend.GameData.Get("tableName", new Where());

if(bro.IsSuccess()) {

} else {
  if(bro.IsTooManyRequestError()) {
    Debug.Log("단기간에 너무 많은 요청을 보낼 경우");
  }
}
```
