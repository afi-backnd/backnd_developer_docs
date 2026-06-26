---
sidebar_label: 과다 호출 에러 핸들러 (Local)
sidebar_position: 5
---

# OnTooManyRequestByLocalError

## 설명

초당 1회 이상 호출 등 과도한 요청으로 서버에서 차단된 후에도 계속 요청을 보낼 경우, 자동으로 호출되는 핸들러입니다.  
다음과 같은 에러가 리턴될 경우 호출됩니다.  

**과도한 요청이 지속될 경우**  
statusCode : 403  
errorCode : Forbidden  
message : 403 Forbidden by Local

## Example

### 메인 쓰레드

```js
void Start() {
    if(Backend.IsInitialized) {        
        Backend.ErrorHandler.OnTooManyRequestByLocalError = () => {
            Debug.Log("403 로컬 에러 발생!!!");
            gameObject.GetComponentInChildren<Text>().text = "과도한 요청중입니다.";
        };
    }
}
```
