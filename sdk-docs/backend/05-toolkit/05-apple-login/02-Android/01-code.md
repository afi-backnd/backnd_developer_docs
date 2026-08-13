---
sidebar_label: "애플 로그인 코드 예제"
description: "AppleLogin"
---

# AppleLogin  
public bool **AppleLogin**(string **webClientId**, out string **errorMessage**, bool **isCallbackInMainThread**, AppleLoginCallback **callback**);  
public bool **AppleLogin**(string **webClientId**, out string **errorMessage**, bool **isCallbackInMainThread**, int **left**, int **top**, int **right**, int **bottom**, AppleLoginCallback **callback**);  



## 파라미터
| Value        | Type           | Description  |  Default |
| :------------ |:-------------| :----- |  :----- |
| identifier      | string | [Certificates, Identifiers & Profiles 페이지](https://developer.apple.com/account/resources/identifiers/list/serviceId) > Identifiers > Services IDs에서 생성한 Identifier | -  | 
| errorMessage    | out String | 성공 시, 빈 값 / 실패 시, 에러정보 |  -  |
| isCallbackInMainThread    | bool | true일 경우, 콜백이 유니티 메인쓰레드에서 작동합니다. false일 경우, 외부쓰레드에서 작동합니다. |  -  |
| left    | int | 왼쪽 여백 px |  0  |
| top    | int | 위쪽 여백 px |  0  |
| right    | int | 오른쪽 여백 px |  0  |
| bottom    | int | 아래쪽 여백 px |  0  |
| callback    | AppleLoginCallback | 애플 로그인 성공 시 호출되는 콜백 함수 | - |

애플 로그인을 시도합니다. 결과는 콜백 이벤트를 통해 확인할 수 있습니다.  
**identifier**은 아래와 같은 값입니다.  
![](/img/docs/guide/toolkit/apple-login/copy-identifiers.png)

:::danger 메인쓰레드와 외부쓰레드
유니티에서 외부쓰레드에서 UnityEngine 혹은 UnityEngine.UI로 생성된 오브젝트나 함수를 호출할 경우, Exception이 발생합니다.
s
메인쓰레드로 콜백을 호출할 경우, 콜백에서 Object를 많이 생성하거나 동기 형식의 뒤끝 함수등의 게임이 잠시 멈추거나 무거운 동작을 할 경우, 크러쉬 현상이 발생할 수 있습니다.
:::

### AppleLoginCallback
토큰이 발급되었을 경우, 자동으로 호출되는 핸들러입니다.  AppleLogin의 변수로 할당 가능합니다.

```js
public delegate void AppleLoginCallback(string token);
private static AppleLoginCallback OnAppleLogin;
```

### OnCloseButtonClick
애플 로그인을 우측 상단에 버튼으로 종료하였을 경우, 호출되는 핸들러입니다.

```js
public delegate void CloseButtonClickCallback();
public static CloseButtonClickCallback OnCloseButtonClick;
```


### Example
```js
    public void StartAppleLogin() {
        #if UNITY_ANDROID || UNITY_EDITOR
        TheBackend.ToolKit.AppleLogin.Android.OnCloseButtonClick = () => {
            Debug.Log("창이 닫혔습니다");
        };
        
        TheBackend.ToolKit.AppleLogin.Android.AppleLogin("com.thebackend.testapp.applelogin", out var error, true, token => {
            Debug.Log("토큰 : " + token);            
            Debug.Log("토큰 발급이 완료되었습니다. 로그인이 가능합니다.");

            // 경고! : 다음과 같이 동기 함수를 호출하지 마세요
            // var bro = Backend.BMember.AuthorizeFederation(token, FederationType.Apple);

            // 아래와 같이 비동기 함수를 호출해주세요,
            Backend.BMember.AuthorizeFederation(token, FederationType.Apple, callback => { 
                Debug.Log("애플 로그인 결과 : " + callback); 
            });
        });

        if (string.IsNullOrEmpty(error) == false) {
            Debug.Log("에러 : " + error);
        }
        #endif
    }
```

### error cases

**(AuthorizeFederation함수) 입력한 identifier와 콘솔에서 설정한 identifier의 값이 올바르지 않는 경우**  
statusCode : 401  
errorCode BadUnauthorizedException  
message bad bad appId, 잘못된 bad appId입니다  
