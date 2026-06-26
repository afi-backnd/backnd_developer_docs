# 에러 정보 확인

뒤끝의 에러 정보는 **ErrorCode**, **StatusCode**, **Message** 3가지로 구성되어 있습니다.  
서버에서 에러가 리턴되었을 때 위 3가지 정보를 이용하여 에러를 확인 및 적절히 처리할 수 있습니다.  

## GetStatusCode
public string **GetStatusCode**();

### 설명
응답 스테이터스 코드를 확인합니다.  

```js
var bro = Backend.GameData.Get("tableName", "rowIndate");
var statusCode = bro.GetStatusCode();
```

### Return Cases
**요청이 성공한 경우**  
요청 종류에 따라 200, 201, 204 가 리턴됩니다.  

**요청이 실패한 경우**  
300 이상의 숫자가 리턴됩니다.  

## GetErrorCode
public string **GetErrorCode**();

### 설명
응답 에러 코드를 확인합니다.  

### Example
```js
var bro = Backend.GameData.Get("tableName", "rowIndate");
var errorCode = bro.GetErrorCode();
```

### Return Cases
**요청이 성공한 경우**  
null

**요청이 실패한 경우**  
에러 정보

## GetMessage
public string **GetMessage**();

### 설명
응답 메시지를 확인합니다.  

```js
var bro = Backend.GameData.Get("tableName", "rowIndate");
var msg = bro.GetMessage();
```

### Return Cases
**요청이 성공한 경우**  
Success

**요청이 실패한 경우**  
에러의 세부 정보
