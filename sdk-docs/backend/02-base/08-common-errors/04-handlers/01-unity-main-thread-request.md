---
sidebar_label: "유니티 메인쓰레드에서 핸들러 호출"
sidebar_position: "1"
description: "유니티 메인 쓰레드에서 핸들러 호출"
---

# 유니티 메인 쓰레드에서 핸들러 호출

기존 에러 핸들러는 외부 쓰레드에서 응답을 받아 에러 체크를 처리하였지만 5.12.0 이후 버전부터는 메인쓰레드에서 서버 요청이 작동하기에, 더이상 Poll을 선언하지 않아도 됩니다.  

## Example

```js
public class BackendErrorManager : MonoBehaviour {

    void Start() {

        if(Backend.IsInitialized) {
            Backend.ErrorHandler.OnMaintenanceError = () => {
                Debug.Log("점검 에러 발생!!!");
                gameObject.GetComponentInChildren<Text>().text = "점검 중입니다.";
            };
            Backend.ErrorHandler.OnTooManyRequestError = () => {
                Debug.Log("403 에러 발생!!!");
                gameObject.GetComponentInChildren<Text>().text = "과도한 요청이 감지되었습니다.";
            };
            Backend.ErrorHandler.OnTooManyRequestByLocalError = () => {
                Debug.Log("403 로컬 에러 발생!!!");
                gameObject.GetComponentInChildren<Text>().text = "과도한 요청중입니다.";
            };
            Backend.ErrorHandler.OnOtherDeviceLoginDetectedError = () => {
                Debug.Log("리프레시 불가!!!");
                gameObject.GetComponentInChildren<Text>().text = "다른 기기에서 로그인이 감지되었습니다.";
            };
        }
    }
}
```
