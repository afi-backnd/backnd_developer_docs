---
sidebar_label: 다른 기기 접속 감지 핸들러
description: "다른 기기 접속 감지 핸들러"
sidebar_position: 2
---

# OnOtherDeviceLoginDetectedError

## 설명

다른 기기에서 현재 접속 중인 계정으로 로그인하면, 기존 기기의 토큰이 만료되어 모든 요청이 실패합니다.
이때 다음과 같은 에러가 리턴되며, 핸들러가 호출됩니다.  
다음과 같은 에러가 리턴될 경우 호출됩니다.  

**다른 기기로 로그인하여 refresh_token이 만료된 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad refreshToken, 잘못된 refreshToken 입니다  

:::danger 핸들러 작동에 따른 안내 문구 설정 주의사항  
다른 기기 접속을 통해 토큰이 만료되는 경우 외에,  
유저가 접속중인 상황에서 콘솔을 통해 제재(차단)을 한 경우에도 토큰이 만료되어 핸들러가 작동할 수 있습니다.  
이러한 상황을 고려한다면 핸들러 작동시 유저에게 안내되는 UI의 안내 문구를  
**'로그인 정보가 만료(중복 로그인, 제재 등)되었습니다, 게임을 다시 시작해 주세요.'** 와 같이 여러 상황을 안내할 수 있는 문구 형태로 적용하시는 것을 권장드립니다.  
:::    

해당 핸들러가 작동하기 위해서는 뒤끝 인스펙터창에 Auto Refresh Token 칸이 활성화되어 있어야 합니다.  

<img src="https://developer.thebackend.io/static/img/unity/error/handler/refresh.png" />

## Example

```js
void Start() {
    if(Backend.IsInitialized) {
        Backend.ErrorHandler.OnOtherDeviceLoginDetectedError = () => {
            Debug.Log("외부 로그인 감지!!!");
            gameObject.GetComponentInChildren<Text>().text = "다른 기기에서 로그인이 감지되었습니다.";
        };
    }
}
```

