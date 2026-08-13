---
sidebar_label: "디바이스 차단"
sidebar_position: "7"
description: "IsDeviceBlockError"
---

# IsDeviceBlockError

## 설명

사용중인 기기가 뒤끝 콘솔의 디바이스 차단 목록에 등록되었을 경우 발생하는 403 blocked device 에러 발생 시, true가 리턴됩니다.  

포함되는 에러케이스는 다음과 같습니다.  

- **디바이스가 차단되었을 경우**  
  statusCode : 403  
  errorCode : ForbiddenException  
  message : Forbidden blocked device, 금지된 blocked device

## Example

```js
BackendReturnObject bro = Backend.GameData.Get("tableName", new Where());
if(bro.IsSuccess() == false) {
    if(bro.IsBadAccessTokenError()) {
        var bro2 = Backend.BMember.RefreshTheBackendToken();

        if(bro2.IsSuccess() == false && bro2.IsDeviceBlockError()) {
            Debug.Log("해당 디바이스는 차단되었습니다.");
        }
    }
}
```
