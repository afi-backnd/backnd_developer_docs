---
sidebar_label: "[New] Line 로그인 인증 예제"
sidebar_position: "4.7"
description: "LINE SDK for Unity 사용"
---

# LINE SDK for Unity 사용

#### Android 및 iOS 플랫폼에서 LINE Unity SDK를 사용하여 뒤끝에 로그인 합니다.  

:::info Flow of the Process

1. Line 로그인 기능을 통해 라인 계정으로 로그인  
2. 라인 계정으로 로그인하여 라인 idToken 획득  
3. 라인 idToken 검증을 통해 뒤끝 Access token 획득  
4. 뒤끝 Access token을 통해 뒤끝 서버에 로그인  
:::

:::tip Line SDK 동작 환경
- Line 로그인 기능은 아래의 환경 이상에서 동작합니다.  
  - Unity 2020.3.15 이상, iOS 및 Android 모듈이 설치된 경우  
  - iOS 13.0 이상, Xcode 14.1 이상  
  - Android Target API Level 33 (Android 13) 이상  
:::  

## 초기 설정

:::note 라인 개발자 계정 설정
라인 개발자 콘솔에 로그인 하기 위해서는 라인 비즈니스 계정이 필요합니다.
:::
### 1. 라인 개발자 콘솔 접속 및 채널 생성
-  **[라인 개발자 콘솔](https://developers.line.biz/console/)**에 접속하여 로그인하고, **Create a new channel**을 클릭하여 **Line Login 채널을 생성**합니다.
![image](/img/docs/guide/base/federation/line_create_channel.png)
### 2. Line Login 채널 생성 설정
- 앱 생성시 App Type 항목에서 **Web app, Mobile app**에 모두 체크하고 채널을 생성합니다.
![image](/img/docs/guide/base/federation/line_app_type_checkbox.png)
### 3. 채널 생성 완료
- 생성이 완료되면 생성된 채널을 클릭합니다.
![image](/img/docs/guide/base/federation/line_complete_create_channel.png)
- Basic Settings 탭을 클릭합니다.
![image](/img/docs/guide/base/federation/line_channel_basic_settings.png)
### 4. OpenID Connect 적용
- Basic Settings 탭의 하단에 위치한 OpenID Connect를 Apply 버튼을 클릭합니다.  
![image](/img/docs/guide/base/federation/line_openid_apply.png)
- 라인에 로그인한 유저의 이메일 정보를 가져오기 위해 요구사항에 알맞게 제출하여 OpenID Connect를 활성화 시킵니다.
![image](/img/docs/guide/base/federation/line_oc_submit.png)
- OpenID Connect 활성화 후 Permissions에 OC_EMAIL이 추가 되었는지 확인 합니다.
![image](/img/docs/guide/base/federation/line_check_permission.png)
### 5. Use LINE Login in your mobile app 활성화
- 최상단으로 돌아가 **LINE Login**을 클릭하여 **Use LINE Login in your mobile app** 부분을 활성화 합니다.  
![image](/img/docs/guide/base/federation/line_move_login_tab.png)

- 항목이 활성화가 되면 iOS는 bundle ID / Android는 Package names를 입력합니다.
![image](/img/docs/guide/base/federation/line_login_mobile_app.png)
### 6. 뒤끝 콘솔 인증 정보에 라인 정보 입력
- 뒤끝 콘솔 인증 정보에 생성한 라인 채널의 Channel ID와 Channel secret값을 적용합니다.

![image](/img/docs/guide/base/federation/line_auth_info.png)
- Channel ID와 Channel secret값은 Basic Settings에서 확인 가능합니다.

![image](/img/docs/guide/base/federation/line_app_id.png)
![image](/img/docs/guide/base/federation/line_sceret_key.png)

## Line SDK 설치 및 적용
### 1. 라인 SDK 유니티패키지 설치
- 라인에서 제공하는 [라인 SDK 유니티패키지](https://github.com/line/line-sdk-unity/releases)파일을 다운 받아 사용중인 유니티 프로젝트에 라인 SDK를 설치 합니다.
### 2. LineSDK 프리팹 설정
- Assets > LineSDK폴더에 존재하는 LineSDK 프리팹을 드래그 또는 복사하여 하이어라키에 위치 시킵니다.
![image](/img/docs/guide/base/federation/line_unity_line_prefab.png)
- 하이어라키에 있는 LineSDK 프리팹을 클릭하여 나타나는 인스펙터창에 생성한 라인 채널의 Basic Settings에 있던 Channel ID를 입력합니다.
![image](/img/docs/guide/base/federation/line_unity_input_appid.png)
### 3. 구현 및 확인
- 예제 코드를 참고해서 로직을 완성 시키고 Android/iOS 환경에서 빌드하여 로그인을 실행합니다.

---

## Line SDK iOS 환경 설정

iOS 환경에서 라인 SDK를 빌드하여 사용하기 위해서는 몇가지 설정이 필요합니다.  

:::warning iOS 빌드 시 주의 사항
iOS 환경에서 아래의 작업을 하지 않고 빌드를 하게되면 빌드 에러가 발생할 가능성이 있습니다.  
빌드 시 에러가 발생하면 아래의 설정을 따라주세요.  
:::

### 1. LineSDKObjC.xcframework 설정  
iOS환경에서 유니티 `Asset > Plugins > iOS > LineSDK > vendor` 폴더로 이동하면 `LineSDKObjC.xcframework` 가 있습니다.  
`LineSDKObjC.xcframework`를 클릭해서 보면 아래 이미지 처럼 나타납니다.  

![image](/img/docs/guide/base/federation/line_ios_xcfromework.png)

아래 설명하는 두가지 방법 중 하나 또는 전부 설정하시기 바랍니다.   

1) LineSDKObjC.xcframework > Info.plist 파일 수정  
**`LineSDKObjC.xcframework`** 내에 존재하는 **`Info.plist`** 파일을 열어  
`AvailableLibraries > Item 1` 항목을 마이너스(-) 버튼을 눌러 삭제합니다.  

![image](/img/docs/guide/base/federation/line_ios_xcfromework_plist.png)

2) ios-arm64_x86_64-simulator > LineSDKObjC.framework 수정  
**`LineSDKObjC.xcframework`** 내에 **`ios-arm64_x86_64-simulator`** 폴더가 존재합니다.  
이 폴더 안에 있는 **`LineSDKObjC`**를 클릭합니다.  
![image](/img/docs/guide/base/federation/line_ios_xcfromework_simulator_1.png)

클릭하면 인스펙터 창이 나타나는데 이미지와 같이 iOS부분에 체크되어 있는 것을 해제하고 apply합니다.  
![image](/img/docs/guide/base/federation/line_ios_xcfromework_simulator_2.png)


### 2. Info.plist 설정  

:::info Info.plist 확인
해당 plist 파일은 위에서 설정한 LineSDKObjC.xcframework에 위치한 파일과는 다른 파일입니다.  
파일을 수정 하기전에 확인해주세요.
:::
:::info 참고 사항
아래의 설정은 빌드 에러가 발생하지는 않지만 웹뷰 또는 라인앱에서 로그인 이후 다시 원래 앱으로 돌아올 수 있도록 하는 설정입니다.
:::
iOS환경에서 빌드한 폴더 내에 만들어지는 Info.plist 파일을 [**라인 공식 iOS 환경 설정**](https://developers.line.biz/en/docs/line-login-sdks/ios-sdk/swift/setting-up-project/#linking-app-to-channel) 에 따라서 파일을 구성합니다.  

파일을 열어서 `CFBundleURLTypes`부분을 찾아 아래 이미지 같이 작성합니다.  
![image](/img/docs/guide/base/federation/line_ios_build_plist.png)
`LSApplicationQueriesSchemes`도 없다면 같이 추가해야합니다.  
![image](/img/docs/guide/base/federation/line_ios_build_plist_1.png)

### Info.plist 예시
```js
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <!-- Specify URL scheme to use when returning from LINE to your app. -->
            <string>line3rdp.$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <!-- Specify URL scheme to use when launching LINE from your app. -->
    <string>lineauth2</string>
</array>
```
---

# AuthorizeFederation

public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**);  
public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**, string **ect**);

## 파라미터

| Value           | Type           | Description                                                 |
| --------------- | -------------- | ----------------------------------------------------------- |
| federationToken | string         | 각 로그인 플러그인을 통해 생성된 token 값                   |
| federationType  | FederationType | 페더레이션의 종류.(FederationType.Line)                    |
| ect             | string         | (Optional) 부가적으로 나오는 정보들 중에 저장하고 싶은 정보 |

## 설명

애플의 회원정보 토큰 값을 이용하여 회원가입/로그인을 시도합니다.

## 일반 코드 Example

```js
public void StartLineFederation()
{
    Debug.Log("Start Line Federation");

    var scopes = new string[] { "profile", "openid", "email"/*, "offline_access"*/ };

    LineSDK.Instance.Login(scopes, result =>
    {
        result.Match(loginResult =>
        {
            // 로그인 성공 시 이 부분이 실행됩니다.
            Debug.Log("LINE 로그인 성공!");
            Debug.Log($"UserId : {loginResult.UserProfile.UserId}");
            Debug.Log($"User Display Name : {loginResult.UserProfile.DisplayName}");
            Debug.Log($"Status Message : {loginResult.UserProfile.StatusMessage}");
            Debug.Log($"Picture Url : {loginResult.UserProfile.PictureUrl}");

            Debug.Log("뒤끝 서버 라인 계정 로그인");
            var loginBro = Backend.BMember.AuthorizeFederation(loginResult.AccessToken.IdTokenRaw, FederationType.Line);
            if (loginBro.IsSuccess())
            {
                Debug.Log("라인 로그인 페더레이션 성공 : " + loginBro);
                Backend.BMember.UpdateFederationEmail(loginResult.UserProfile.UserId, FederationType.Line);
                BackendInit.instance.SetFederationName("라인 로그인");
            }
            else
            {
                Debug.Log("라인 로그인 페더레이션 실패 : " + loginBro);
            }
        },
        error =>
        {
            //라인 로그인 실패 시 이 부분이 실행됩니다.
            Debug.LogError("LINE 로그인 실패!");
            Debug.LogError("에러: " + error.Message);
        }
      );
    });
}
```

## ReturnCase

### Success cases  
**로그인에 성공한 경우**  
statusCode : 200  
message : Success  

**신규 회원가입에 성공한 경우**  
statusCode : 201  
message : Success  

### Error cases  
**차단당한 계정일 경우**  
statusCode : 403  
errorCode : 콘솔에서 입력한 차단된 사유  
message : forbidden blocked user, 금지된 blocked user 

**뒤끝 콘솔에 입력한 라인 정보가 잘못되었을 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
Message : bad [LINE] invalid_request, 잘못된 [LINE] invalid_request 입니다

---
