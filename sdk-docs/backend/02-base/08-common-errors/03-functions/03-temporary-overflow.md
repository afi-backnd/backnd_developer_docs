---
sidebar_label: 서버의 일시적인 과부하
sidebar_position: 3
---

# IsServerError

public bool **IsServerError**();

## 설명

서버가 일시적으로 과부화 상태이거나 정상적으로 발생하지 않는 500, 502, 503 에러가 발생할 경우 true가 반환됩니다.  

포함되는 에러케이스는 다음과 같습니다.  

**서버가 일시적으로 과부화 상태일 경우**  
statusCode : 500  
errorCode : InternalServerError  
message : {"message":"Request failed with status code 502"} 또는 {"message":"Request failed with status code 504"}

**서버가 일시적으로 과부화 상태일 경우**  
statusCode : 502  
errorCode : BadGateway  
message : 502 Bad Gateway

**요청에 대한 시간이 오래걸릴 경우**  
statusCode : 503  
errorCode : ETIMEDOUT  
message : Response timeout

**서버가 정상적으로 작동하지 않는 경우**  
statusCode : 503  
errorCode : Service Temporarily Unavailable  
message : 503 Service Temporarily Unavailable

유저의 유입이 한번에 많아질 때 발생할 수 있으며, 30초 혹은 60초 정도 뒤에 동일한 함수를 호출할 경우 성공할 가능성이 높습니다.  

## Example

```js
BackendReturnObject bro = Backend.GameData.Get("tableName", new Where());

if(bro.IsSuccess()) {

} else {
    if(bro.IsServerError()) {
       Debug.Log("서버가 과부화상태이거나 불안정할 경우 발생합니다.");
    }
}
```
