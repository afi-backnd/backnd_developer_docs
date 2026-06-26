---
sidebar_label: 클라이언트 요청 실패
description: "클라이언트 요청 실패"
sidebar_position: 2
---

# IsClientRequestFailError

public bool **IsClientRequestFailError**();

## 설명

네트워크 연결이 불안정하여 아래와 같은 에러가 발생할 경우, true가 반환됩니다.  

포함된 에러케이스는 다음과 같습니다.  

- **네트워크의 상태가 일시적으로 불안정하여 호출/응답에 실패할 경우**  
  statusCode : 400  
  errorCode : HttpRequestException  
  message : An error occurred while sending the request(2)  
  Error getting response stream(ReadDone2): ReceiveFailure

네트워크가 일시적으로 끊기거나 불안정할 때 요청을 보낼 경우 해당 에러가 발생할 수 있으며 다시 한번 호출하면 정상적으로 이루어질 가능성이 높습니다.  

## Example

```js
BackendReturnObject bro = Backend.GameData.Get("tableName", new Where());

if(bro.IsSuccess()) {

} else {
  if(bro.IsClientRequestFailError()) {
    Debug.Log("네트워크가 일시적으로 끊어졌을 경우");
  }
}
```
