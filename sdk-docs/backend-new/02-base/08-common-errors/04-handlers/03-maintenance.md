---
sidebar_label: 서버 점검 에러 핸들러
description: "서버 점검 에러 핸들러"
sidebar_position: 3
---

# OnMaintenanceError

## 설명

[뒤끝 콘솔 - 서버 설정 - 프로젝트]에서 프로젝트 상태를 점검 중으로 변경한 후, 함수를 호출할 경우 자동으로 호출되는 핸들러입니다.  
다음과 같은 에러가 리턴될 경우 호출됩니다.  

**뒤끝 콘솔에서 프로젝트 상태가 점검인 경우(로그인 시)**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad serverStatus: maintenance, 잘못된 serverStatus: maintenance입니다

**뒤끝 콘솔에서 프로젝트 상태가 점검인 경우(로그인 외 기능 호출 시)**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad bad,serverStatus:,maintenance,,잘못된,serverStatus:,maintenance,입니다, 잘못된 bad,serverStatus:,maintenance,,잘못된,serverStatus:,maintenance,입니다 입니다

## Example

### 메인 쓰레드

```js
void Start() {
    if(Backend.IsInitialized) {        
        Backend.ErrorHandler.OnMaintenanceError = () => {
            Debug.Log("점검 에러 발생!!!");
            gameObject.GetComponentInChildren<Text>().text = "점검 중입니다.";
        };
    }
}
```