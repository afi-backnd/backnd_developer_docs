---
description: "기기 차단 에러 케이스"
---

# 기기 차단 에러 케이스

## 개요

기기 차단 에러의 절차는 다음과 같습니다.  

1. 게임 이용 도중 기기 차단 발생
2. 해당 기기를 이용중인 유저의 액세스 토큰 제거
3. 모든 뒤끝 함수 호출에 bad accesstoken 에러 발생
4. 액세스 토큰 재발급을 위해 RefreshTheBackendToken 혹은 로그인 함수 호출
5. 토큰 재발급 혹은 로그인 함수에서 403 Forbidden blocked device 발생

## 설명

뒤끝 콘솔을 통해 디바이스 차단에 등록된 기기를 이용하는 유저는 로그인 시 403 에러가 발생합니다.  

statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden blocked device, 금지된 blocked device

만약 해당 유저가 플레이중이라면 콘솔에서 디바이스 차단이 등록된 즉시 액세스토큰이 제거되며 모든 함수에서 아래와 같은 bad accessToken 에러가 출력됩니다.  

- 유저의 Access Token이 올바르지 않거나 만료된 경우  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad bad,accessToken,,잘못된,accessToken,입니다, 잘못된 bad,accessToken,,잘못된,accessToken,입니다 입니다

- 로그인 시, 유저의 Access Token이 올바르지 않거나 만료된 경우  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad accessToken, 잘못된 accessToken입니다

그로 인해 유저는 토큰을 재발급 받거나 재로그인을 통해 액세스 토큰을 발급받아야만 정상적으로 함수 호출이 가능해집니다.  
다만 위의 설명과 같이 차단된 디아비스에서 로그인 시 403 에러가 발생하기 때문에 유저는 로그인이 불가능하게 됩니다.  

이로 인해 게임 이용 도중에도 기기를 차단할 경우에는 유저는 즉시 게임이 불가능하도록 만들 수 있습니다.  

따라서 개발자는 badaccessToken 발생 후 이루어지는 토큰 재발급 함수에서 디바이스 차단 에러가 발생하였는지 확인하는 로직이 필요합니다.  

## 로직 구성

기기 차단에 대한 예외 처리를 할 경우, 모든 뒤끝 함수에 대해 디바이스 차단 에러 케이스를 체크하는 것 보다는 로그인 함수에서만 에러를 확인하는 것이 바람직합니다.  

기기가 도중에 차단된 경우에는 bad accesstoken 에러가 발생하고, Backend.BMember.RefreshTheBackendToken()을 호출하여 토큰을 재발급하는 경우도 로그인 행위에 포함되므로 토큰 재발급시에도 해당 에러를 체크해주시기 바랍니다.  

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

발생한 에러가 디바이스 차단 에러인지 확인하는 함수는 다음과 같습니다.  

- BackendReturnObject 에러 확인 함수 : [IsDeviceBlockError](/sdk-docs/backend/base/common-errors/functions/block-device)

혹은 뒤끝 인스펙터(유니티 상단 TheBackend 메뉴 > Edit Settings)에서 Auto Refresh Token이 체크되어있을 경우에는 위와 같은 로직 없이 핸들러 한번만 설정하여도 디바이스 차단을 구현할 수 있습니다.  

```js
void Start() {
    Backend.ErrorHandler.OnDeviceBlockError= () => {
            Debug.Log("디바이스 차단 발생!");
        };
}

void GetData(){
    BackendReturnObject bro = Backend.GameData.Get("tableName", new Where());
}
```

해당 핸들러는 다음과 같은 행동으로 동작합니다.  

1. 뒤끝 함수가 호출될 때마다 401 bad accesstoken 발생 검사
2. 발생되었을 경우 토큰 재발급 함수 호출
3. 제발급 함수의 리턴케이스에서 403 디바이스 차단 에러 발견 시 핸들러 호출

- 에러 발생시 호출되는 핸들러 : [OnDeviceBlockError](/sdk-docs/backend/base/common-errors/functions/block-device)
