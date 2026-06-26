---
sidebar_label: 과다 호출 에러 핸들러 (Server)
description: "과다 호출 에러 핸들러 (Server)"
sidebar_position: 5
---

# OnTooManyRequestError

## 설명

초당 1회 이상 호출 등 과도한 요청을 보낼 경우, 서버에서 요청이 차단되며 자동으로 호출되는 핸들러입니다.  
과도한 요청이 감지되고 5분 30초가 지난 후에 다시 정상적으로 호출됩니다.  

다음과 같은 에러가 리턴될 경우 호출됩니다.  

**한 클라이언트(동일 ip)에서 너무 많은 요청을 보낸 경우**  
statusCode : 403  
errorCode : Forbidden  
message : 403 Forbidden

## Example

### 메인쓰레드

```js
void Start() {
    if(Backend.IsInitialized) {
        Backend.ErrorHandler.OnTooManyRequestError = () => {
            Debug.Log("403 에러 발생!!!");
            gameObject.GetComponentInChildren<Text>().text = "과도한 요청이 감지되었습니다.";
        };
    }
}
```
