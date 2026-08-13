---
sidebar_label: "디바이스 차단 에러 핸들러"
sidebar_position: "6"
description: "OnDeviceBlockError"
---

# OnDeviceBlockError

## 설명

뒤끝 콘솔에서 게임 접근 관리 - 접근 차단에서 차단 디바이스 등록 시 자동으로 호출되는 핸들러 입니다.  
다음과 같은 에러가 리턴될 경우 호출됩니다.  

**디바이스가 차단되었을 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden blocked device, 금지된 blocked device

## Example

### 메인 쓰레드

```js
void Start() {
    if(Backend.IsInitialized) {
        Backend.ErrorHandler.OnDeviceBlockError= () => {
            Debug.Log("디바이스 차단 발생");
            gameObject.GetComponentInChildren<Text>().text = "해당 디바이스가 차단되었습니다.";
        };
    }
}
```
