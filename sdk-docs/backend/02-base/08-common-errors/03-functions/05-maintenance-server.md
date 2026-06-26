---
sidebar_label: 서버 상태 점검중
sidebar_position: 5
---

# IsMaintenanceError

public bool **IsMaintenanceError**();

## 설명

서버가 점검상태일 경우 true가 반환됩니다.  

포함되는 에러케이스는 다음과 같습니다.  

- **뒤끝 콘솔에서 프로젝트 상태가 점검인 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad bad,serverStatus:,maintenance,,잘못된,serverStatus:,maintenance,입니다, 잘못된 bad,serverStatus:,maintenance,,잘못된,serverStatus:,maintenance,입니다 입니다

- **로그인 시, 뒤끝 콘솔에서 프로젝트 상태가 점검인 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad serverStatus: maintenance, 잘못된 serverStatus: maintenance입니다

네트워크가 일시적으로 끊기거나 불안정할 때 요청을 보낼 경우 해당 에러가 발생할 수 있으며 다시 한번 호출하면 정상적으로 이루어질 가능성이 높습니다.  

## Example

```js
BackendReturnObject bro = Backend.GameData.Get("tableName", new Where());

if(bro.IsSuccess()) {

} else {
  if(bro.IsMaintenanceError()) {
    Debug.Log("서버가 점검중일 경우");
  }
}
```
