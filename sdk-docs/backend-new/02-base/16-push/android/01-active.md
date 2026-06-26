---
sidebar_label: 푸시 등록
---

# RegisterPush
public Task&lt;RequestResult&gt; **RegisterPushAsync**(string **deviceToken**);


## 파라미터

| Value|  Type | Description |
| --- | --- | --- |
| deviceToken | string | 로컬 기기에 대한 정보. |

## 설명
푸시 설정을 ON 합니다.  
푸시 설정이 ON일 경우, 뒤끝 콘솔에서 푸시 기능을 사용할 시 푸시 알람이 옵니다.  
푸시는 기본적으로 **야간 푸시 수신 미동의**로 설정되어 있습니다. 이를 변경하고자 할 경우에는 [SetNightPushEnabled 함수](/sdk-docs/backend/base/push/android/night-push)를 참고해주세요.  
푸시 등록에 관해서는 [안드로이드 푸시 등록 가이드](/guide/console-guide/server-setting/push/for-android)를 참고해 주세요.  


## Example


### 디바이스 토큰 불러오기 코드 작성
```js
using Firebase;
using Firebase.Messaging;


public class PushTest : MonoBehaviour
{

    string token = string.Empty;

    void Start() {
        FirebaseApp.CheckAndFixDependenciesAsync().ContinueWith(task =>
        {
            var dependencyStatus = task.Result;
            if (dependencyStatus == DependencyStatus.Available)
            {
                // Firebase 초기화
                FirebaseApp app = FirebaseApp.DefaultInstance;
                // FCM 토큰 가져오기
                GetToken();
            }
            else
            {
                Debug.Log($"Could not resolve all Firebase dependencies: {dependencyStatus}");
            }
        });
    }

    void GetToken()
    {
        FirebaseMessaging.TokenReceived += OnTokenReceived;

        // 해당 task는 외부쓰레드로 작동합니다.
        // 만약 GameObject.Instantiate 같은 유니티 함수나 UnityEngine.UI를 사용할 경우, 예외가 발생합니다.
        FirebaseMessaging.GetTokenAsync().ContinueWith(task =>
        {
            if (task.IsCompleted)
            {
                token = task.Result;
                Debug.Log($"FCM Token: {token}");

                BackndPushNotification.Android.RegisterPush(token, callback =>
                {
                    Debug.Log("푸시 등록 여부 : " + callback);
                });
            }
            else
            {
                Debug.Log("Failed to get FCM token");
            }
        });
    }

    void OnTokenReceived(object sender, TokenReceivedEventArgs token)
    {
        Debug.Log($"Received Registration Token: {token.Token}");
    }
    
}

```

### Task 방식
```js
string token = "";
var reqResult = await BackndPushNotification.Android.RegisterPushAsync(token);
```

### Callback 방식
```js
BackndPushNotification.Android.RegisterPush(token, (callback) => 
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**등록에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**잘못된 푸시 토큰 정보일 경우(string.Empty, null등)**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined deviceToken, deviceToken을(를) 확인할 수 없습니다

>파이어베이스 sdk 혹은 google-services.json 파일이 잘못 설치된 경우, 에러가 발생할 수 있습니다.  
Backend.Android.GetDeviceToken()을 통해 값이 난수로된 string 값인지 확인해주세요.  
string.Empty일 경우, 아래와 같은 동작을 통해 파이어베이스 sdk 혹은 json파일을 재설치해주세요.  
* FirebaseMessaging.unitypackage 다시 임포트
* 플러그인 정리(Assets > External Dependency Manager > Android Resolver > Force Resolve)
* 파이어베이스의 패키지네임 설정과 유니티의 패키지네임 설정이 일치하는지 확인
* 파이어베이스에서 제공하는 google-services.json 파일 다시 적용

---

