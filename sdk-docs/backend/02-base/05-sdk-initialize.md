---
sidebar_label: "SDK 초기화"
description: "Initialize"
---

# Initialize
public BackendReturnObject **Initialize**();  
public BackendReturnObject **Initialize**(BackendCustomSetting **settings**);  
public void **InitializeAsync**(Backend.BackendCallback **callback**);  
public void **InitializeAsync**(BackendCustomSetting **settings**, Backend.BackendCallback **callback**);


## 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| settings   | BackendCustomSetting | 뒤끝 SDK를 초기화할 때 사용할 인자 값 객체  |   
| callback  | Action&lt;BackendReturnObject&gt; | 초기화 성공/실패 시 호출할 비동기 함수의 콜백 함수 | 


## BackendCustomSetting
| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| packageName | string | 게임의 패키지 네임 | string.Empty |
| clientAppID   | string | 게임을 구분하는 게임 고유 아이디 | string.Empty |
| signatureKey   | string | 게임 데이터를 서버와 주고받을 시 데이터를 암호화할 때 필요한 키 | string.Empty |
| functionAuthKey  | string | 뒤끝펑션을 사용할 때 인증을 위한 키 | string.Empty |
| isSendLogReport  | bool | 서버로 요청을 보낼 때 서버에 추가로 로그를 저장할지 결정하는 값입니다. | true |
| timeOutSec  | uint | 서버로 요청을 보냈을 때 설정한 시간(초) 동안 응답이 오지 않았을 경우 타임아웃 에러로 처리할 시간을 결정합니다.  
default: 100초, 최소: 30초, 최대: 100초 | 100 |
| useAsyncPoll  | bool | 비동기 함수의 콜백을 풀링 기능을 이용하여 메인 스레드에서 처리할지 여부(네트워크 타입이 HTTP_CLIENT일 경우에만 작동) | false |
| waitUntilInitializeFinish  | bool | 국가 정보 불러오기를 비동기로 호출할 것인지, 혹은 초기화 함수 후에 호출하여 국가정보 불러오기까지 응답을 함께 받을 것인지 여부 | false |
> packageName이 string.Empty 인 경우 sdk를 초기화 할 때 [Application.identifier](https://docs.unity3d.com/ScriptReference/Application-identifier.html) 값을 사용합니다.  

## 설명
뒤끝 SDK를 사용하기 위해서는 반드시 SDK 초기화를 수행해야 합니다.  
초기화는 **게임이 실행되는 동안 1회만** 수행하면 됩니다.  

초기화 함수의 인자 값으로 BackendCustomSetting을 넘길 경우 유니티 인스펙터 창에서 설정한 The Backend Settings의 값은 모두 무시하고,
BackendCustomSettings에 정의된 값으로 SDK 초기화를 시도합니다.  
> The Backend Settings에 정의된 값과 BackendCustomSetting에 정의된 값을 혼용하여 사용할 수 없습니다.  


## Example
### 동기
```js
using BackEnd;

void Start()
{
    // 첫 번째 방법(동기)
    var bro = Backend.Initialize();
    if(bro.IsSuccess())
    {
        // 초기화 성공 시 로직
    }
    else
    {
        // 초기화 실패 시 로직
    } 
}
```

### 동기(SDK 설정값을 직접 선언)
```js
using BackEnd;

void Start()
{
    // 두 번째 방법(동기)
    // SDK 설정값
    BackendCustomSetting settings = new BackendCustomSetting();
    settings.clientAppID = "클라이언트 앱 아이디";
    settings.signatureKey = "시그니처 키";
    settings.functionAuthKey = "뒤끝펑션 키";
    settings.isSendLogReport = true;
    settings.timeOutSec = 100;

    var bro = Backend.Initialize(settings);
    if(bro.IsSuccess())
    {
        // 초기화 성공 시 로직
    }
    else
    {
        // 초기화 실패 시 로직
    } 
}
```

### 비동기 
```js
using BackEnd;

void Start()
{
    // 세 번째 방법(비동기)
    Backend.InitializeAsync(callback => {
        if(callback.IsSuccess())
        {
            // 초기화 성공 시 로직
        }
        else
        {
            // 초기화 실패 시 로직
        }    
    });
}
```

### 비동기(SDK 설정값을 직접 선언)
```js
using BackEnd;

void Start()
{
    // 네 번째 방법(비동기)
    // SDK 설정값
    BackendCustomSetting settings = new BackendCustomSetting();
    settings.clientAppID = "클라이언트 앱 아이디";
    settings.signatureKey = "시그니처 키";
    settings.functionAuthKey = "뒤끝펑션 키";    
    settings.isSendLogReport = true;
    settings.timeOutSec = 100;
    settings.useAsyncPoll = true; 
    settings.autoLoadLocationProperties = false;

    Backend.InitializeAsync(settings, callback => {
        if(callback.IsSuccess())
        {
            // 초기화 성공 시 로직
        }
        else
        {
            // 초기화 실패 시 로직
        }    
    });
}
```

### Return Cases

---
####
Success Case
statusCode : 204  
message : Success  
> 추가로 유니티 디버그 콘솔 창에 **Server setting success**가 출력됩니다.  


---
####
Error Case
**Client App ID 혹은 Signature Key가 잘못된 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : game not found, game을(를) 찾을 수 없습니다

**Client App ID 혹은 Signature Key가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : InitializeFail  
message :  Please Edit the Baceknd Settings
