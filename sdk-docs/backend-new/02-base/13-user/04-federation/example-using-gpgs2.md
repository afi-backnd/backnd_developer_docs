---
sidebar_label: "GPGS V2 인증 예제"
description: "GPGS V2 인증 예제"
sidebar_position: 2.6
---

# Google Play Game Service V2(GPGS2) 사용

Google Play Games Service (GPGS)로 간편하게 로그인하는 기능을 넣어서, 사용자 경험을 향상하세요.  
이 가이드에서는 GPGS 회원가입 및 로그인 기능을 구현하는 방법을 소개합니다.  

:::danger GPGS 및 Sign In with Google 이용 주의사항
**GPGS V1은 구글로부터의 지원이 종료되었습니다.  
GPGS V1을 사용해 오셨거나, 새롭게 사용하시고자 하는 경우 Sign In with Google을 사용해 주세요.**  
   
**GPGS V2 로그인 적용 시 Google Play Games 앱의 업데이트 상태나 캐시 데이터 문제로 인해 정상적으로 이용되지 않을 수 있습니다.  
이용자가 로그인 이상을 호소하는 경우, Google Play Games 앱이 최신 버전인지 확인하고 캐시 삭제 후 재시도하도록 안내해 주시기 바랍니다.  
또한 GPGS V2 로그인은 안드로이드 기기에서만 지원되므로, 보다 안정적인 연동을 위해 구글 로그인을 적용하실 것을 권장드립니다.**

GPGS V2 및 Sign In with Google은 다음과 같은 조건으로 이용이 가능합니다.

| 구분 | GPGS V2 | Sign In with Google |
|---|---|---|
| **로그인 정보** | PGS games_lite 정보 이용 | 구글 계정 정보 이용 |
| **계정간 호환** | GPGS V2 단독 <br /> Sign In with Google 호환 불가 | GPGS V2 호환 불가 |
| **iOS 지원** | 미지원 | 미지원 |
:::

:::info Flow of the Process

1. Google Play Game Service를 통해 구글 계정으로 로그인
2. 구글 Auth Code 획득
3. 이 구글 Access Code를 BackndAuth.Instance.GetGPGS2AccessToken() 함수에 사용
4. 구글 액세스 토큰이 발급
5. 구글 액세스 토큰을 이용해 BackndAuth.Instance.SignInWithProvider("accessToken", AuthProvider.GPGS2) 호출

:::

## 초기 설정

### 1. 구글 플레이에 어플리케이션 등록

[구글 플레이 콘솔](https://play.google.com/apps/publish/) > 앱 만들기 > 출시 > 프로덕션 or(공개, 비공개, 내부) 테스트 > 새 버전 만들기

1. Google Play 앱 서명에서 **계속**을 눌러 앱 서명을 등록하여 **앱 서명 키 인증서**를 발급해 줍니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/1.PNG" />
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/2.PNG" />

2. APK 또는 App Bundle을 업로드하여 **업로드 키 인증서**까지 발급해 줍니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/3.PNG" />

3. 발급된 앱 서명은 왼쪽 메뉴 중 **설정 > 앱 서명**에서 확인할 수 있으며, 이후 구글 게임 서비스를 등록할 때 필요합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/4.PNG" />

### 2. 구글 게임 서비스 등록

1. 왼쪽 메뉴 중 **Play 게임 서비스 > 설정 및 관리 > 설정**에서
   **아니요, 게임에서 Google API를 사용하지 않습니다**를 선택하여 새로운 Play 게임 서비스 프로젝트를 생성합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/5.PNG" />

이후, 사용자 인증 정보에 **OAuth 동의 화면 구성**을 클릭하여 **Google Cloud Platform**으로 들어갑니다.  
 <img src="https://developer.thebackend.io/static/img/unity/gpgs/6.PNG" />
<img src="https://developer.thebackend.io/static/img/unity/gpgs/7.PNG" />

2. Google Cloud Platform에서 **OAuth 동의 화면**을 구성합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Auth-1.PNG" />
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Auth-2.PNG" />
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Auth-3.PNG" />

3. 동의 화면 구성 후, 왼쪽 메뉴에 사용자 인증 정보를 클릭하여 **사용자 인증 정보**를 구성합니다.  
   **사용자 인증 정보 만들기 > OAuth 클라이언트 ID > Android 선택**  
   이 과정을 1번(사진 4)에서 등록한 어플리케이션의 앱 서명 > **앱 서명 인증서와 업로드 인증서 두 가지**를 모두 해줍니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/KEY-1.PNG" />
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/KEY-2.PNG" />

다음과 같이 Google Cloud Platform에 **총 2개의 OAuth 2.0 클라이언트 ID**가 등록되어 있어야 합니다.  
 <img src="https://developer.thebackend.io/static/img/unity/gpgs/KEY-3.PNG" />

4. Play 게임 서비스에 사용자 인증 정보 추가
   4번에 Google Cloud Platform에서 추가한 2개의 Android 사용자 인증 정보를 추가합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Setting-1.PNG" />
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Setting-2.PNG" />

다음과 같이 구글 콘솔에 2개의 사용자 인증정보가 추가되어야 합니다.  
 <img src="https://developer.thebackend.io/static/img/unity/gpgs/Setting-3.PNG" />

### 3. 테스터 설정

GPGS를 테스트하기 위해서는 **구글 콘솔에 테스터가 등록**되어 있어야 합니다.

왼쪽 메뉴 **Play 게임 서비스 > 테스터 > 출시 트랙**에서 **트랙 추가**를 클릭하여 앱을 게시한 트랙을 추가합니다.  
<img src="https://developer.thebackend.io/static/img/unity/gpgs/TESTER-2.PNG" />

이후, 테스터 항목에서 **테스터 추가**를 클릭하여 테스터를 추가합니다.
<img src="https://developer.thebackend.io/static/img/unity/gpgs/TESTER-3.PNG" />

### 4. 유니티 설정

1. Unity Project - Google Play Games Services Plugin 추가  
   <https://github.com/playgameservices/play-games-plugin-for-unity>
   > <b>GPGS 버전 11 다운로드</b> > [current-build] 폴더 > unitypackage import

임포트 후, 유니티 상단에 **Windows > Google Play Games > Setup > Android Setup** 클릭

2. Resources Definition 설정

   **Play 게임 서비스 > 설정 및 관리 > 설정 > 사용자 인증 정보** 우측에 **리소스 보기**를 클릭합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/UNITY-1.PNG" />

Android(XML)을 복사하여 유니티 **GPGS 설정 창 - Resources Definition**에 입력해 줍니다.  
 <img src="https://developer.thebackend.io/static/img/unity/gpgs/UNITY-2.PNG" />

3. Client ID 설정

해당 프로젝트의 **Google Cloud Platform > 사용자 인증 정보 > +사용자 인증 정보 만들기**를 클릭합니다.  
 <img src="https://developer.thebackend.io/static/img/unity/gpgs/UNITY-3.PNG" />

어플리케이션 유형을 **웹 애플리케이션**으로 설정하고 생성합니다.  
 <img src="https://developer.thebackend.io/static/img/unity/gpgs/UNITY-4.PNG" />

생성된 웹 어플리케이션의 클라이언트 ID를 복사하여 **유니티 GPGS 설정 창 - Client ID**에 입력해 줍니다.  
 <img src="https://developer.thebackend.io/static/img/unity/gpgs/UNITY-5.PNG" />

### 5. 뒤끝 콘솔 설정

뒤끝 콘솔에서는 뒤끝 서버와의 통신을 위해 승인된 리디렉션 URI에 **https://auth0.thebackend.io**를 입력해야합니다.  
또한 인증 정보 > 구글 로그인 인증 정보에 GPGS와 연동된 GCP의 web Client ID의 Client ID와 Client Secret을 입력해야합니다.

1. [Google Cloud Platform](https://console.cloud.google.com/apis/credentials) > API 및 서비스 > 사용자 인증 정보 > 웹 클라이언트 선택
   ![](/img/docs/guide/console-google-info/select-webclient-id.png)

2. 승인된 리디렉션 URI에 **https://auth0.thebackend.io**를 추가
   ![](/img/docs/guide/console-google-info/redirect-in-google.png)

   > https://auth0.thebackend.io/google/token는 웹 탈퇴 링크 사용시 등록하는 uri로, gpgs v2 구현시에는 필요하지 않습니다.

3. 클라이언트 ID와 클라이언트 보안 비밀번호를 복사
   ![](/img/docs/guide/console-google-info/client-id-in-google.png)

4. 뒤끝 콘솔 > 인증 정보 > 구글 로그인 인증 정보에 Client ID와 Client Secret을 입력
   ![](/img/docs/guide/console-google-info/backend-console-google-info.png)

### 완료

**ID 토큰을 받기 위한 설정은 모두 끝났습니다.**  
 이제 구글 플레이 콘솔에 등록한 아이디를 통해 auth Code를 받을 수 있습니다.

---

# GetGPGS2AccessToken

public Task&lt;GetGPGS2AccessTokenResult&gt; **GetGPGS2AccessTokenAsync**(string **authCode**);  
public Task&lt;GetGPGS2AccessTokenResult&gt; **GetGPGS2AccessTokenAsync**(string **authCode**, string **redirectUri**);

## 파라미터

| Value       | Type   | Description                                                                                            |
| ----------- | ------ | ------------------------------------------------------------------------------------------------------ |
| authCode    | string | GPGS 로그인 이후, PlayGamesPlatform.Instance.RequestServerSideAccess 함수를 호출하여 얻은 인증 코드 값 |
| redirectUri | string | GCP에서 설정한 해당 webClientId에 대한 redirectUri                                                     |

## 설명

GPGS 로그인 이후, GPGS 플러그인을 이용해 서버에 요청 가능한 인증 코드 값을 받습니다.  
해당 인증 코드 값을 이용하여 페데레이션 로그인에 사용되는 액세스 토큰 값을 가져옵니다.

:::info GPGS V2 로그인 방법
GetGPGS2AccessToken를 통해 액세스토큰을 발급받은 후에는 해당 액세스토큰을 AuthorizeFederation("access_token", FederationType.GPGS2)에 사용하여 로그인을 해야합니다!

AuthorizeFederation에 대한 설명은 아래에 설명되어있습니다.  
:::

## Example

### Task 방식
```js
var google_code = "4/0AeaYSHCuDcNr-Kai7Hx42BpreFACh3wLVEe5u7ZF-phOY8qv6lAHQ2R7a5Y3zWOqkfqocw";
var reqResult = await BackndAuth.Instance.GetGPGS2AccessTokenAsync(google_code);
if (reqResult.IsSuccess())
{
    string accessToken = reqResult.GetAccessToken();
}
```

### Callback 방식
```js
var google_code = "4/0AeaYSHCuDcNr-Kai7Hx42BpreFACh3wLVEe5u7ZF-phOY8qv6lAHQ2R7a5Y3zWOqkfqocw";
BackndAuth.Instance.GetGPGS2AccessToken(google_code, callback =>
{
    if (callback.IsSuccess())
    {
        string accessToken = callback.GetAccessToken();
    }
});
```


## Sample Code

```js
using BACKND.Base;
using GooglePlayGames;
using GooglePlayGames.BasicApi;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{

    void Start()
    {
      PlayGamesPlatform.Instance.Authenticate(ProcessAuthentication);
    }


    void ProcessAuthentication(SignInStatus status)
    {
      if (status == SignInStatus.Success)
      {
        GetAccessCode();
        // Continue with Play Games Services
      }
      else
      {
        // Disable your integration with Play Games Services or show a login button
        // to ask users to sign-in. Clicking it should call
        // PlayGamesPlatform.Instance.ManuallyAuthenticate(ProcessAuthentication).
      }
    }

    public void GetAccessCode()
    {
        PlayGamesPlatform.Instance.RequestServerSideAccess(
          /* forceRefreshToken= */ false,
          async code => {
              Debug.Log("구글 인증 코드 : " + code);

              BackndAuth.Instance.GetGPGS2AccessToken(code, googleCallback =>
              {
                  Debug.Log("GetGPGS2AccessToken 함수 호출 결과 " + googleCallback);

                  string accessToken = "";

                  if (googleCallback.IsSuccess())
                  {
                      accessToken = googleCallback.GetAccessToken();
                  }
              });
          });
    }
}
```

## ReturnCase

### Success cases

**토큰 발급에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue :

```js
{
  "access_token": "ya29.a0AfB_byAGc2xHeWWevNzeLGmzbOKJC5ydCpf_GaboF1PziwI6ZF3i8UqttxPM_sxJu9d76rIVH0IdPGdRLYsHySae-TdmsUUlG9-TNEuZYRQwz8CHsENfcX9hHnzvbA-JqqzLxuglfTwwV_RPvTgORgHxNsxGd5whGgaCgYKAaoSARESFQHGX2Mi7KDxujEBmU_9QeEKMUsw6A0169",
  "expires_in": 3598,
  "scope": "https://www.googleapis.com/auth/games_lite https://www.googleapis.com/auth/drive.appdata",
  "token_type": "Bearer"
}
```

### Error cases

**뒤끝 콘솔에 구글 로그인 정보를 잘못 입력하였을 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : GPGS authorization info not found, GPGS authorization info을(를) 찾을 수 없습니다

**뒤끝 콘솔에 구글 로그인 정보를 잘못 입력하였을 경우**  
statusCode : 400  
errorCode : GoogleOAuthException  
message : invalid_grant : Bad Request

**뒤끝 콘솔에 client secret가 잘못 입력되어있을 경우**  
statusCode : 401  
errorCode : GoogleOAuthException  
message : invalid_client : Unauthorized

**GCP 프로젝트에 승인된 리디렉션 URI를 입력하지 않았을 경우**  
statusCode : 400  
errorCode : GoogleOAuthException  
message : redirect_uri_mismatch : Bad Request

**GCP 프로젝트에 안드로이드 클라이언트를 추가하지 않았을 경우**  
statusCode : 400  
errorCode : GoogleOAuthException  
message : invalid_request : Missing required parameter: code

---

# SignInWithProvider

public Task&lt;RequestResult&gt; **SignInWithProviderAsync**(string **authToken**, AuthProvider **provider**);  
public Task&lt;RequestResult&gt; **SignInWithProviderAsync**(string **authToken**, AuthProvider **provider**, string **ect**);

## 파라미터

| Value           | Type           | Description                                                 |
| --------------- | -------------- | ----------------------------------------------------------- |
| authToken       | string         | 각 로그인 플러그인을 통해 생성된 token 값                   |
| provider        | AuthProvider   | 외부 로그인 제공자의 종류.(AuthProvider.Line)                    |
| ect             | string         | (Optional) 부가적으로 나오는 정보들 중에 저장하고 싶은 정보 |


## 설명

Google Play Game Services의 회원정보 토큰 값을 이용하여 회원가입/로그인을 시도합니다.

:::info GPGS V1, Sign in with Google과 GPGS V2 연동 불가 안내
GPGS V2는 Play Game Services의 정보를 이용해 로그인하는 것이며, Sign in with Google은 구글 계정의 정보를 이용해 로그인을 합니다.  
따라서 GPGS V2에서 로그인을 할 경우, Sign in with Google으로 외부 로그인을 한 유저는 연동이 불가능합니다.
:::

```js
using BACKND.Base;
using GooglePlayGames;
using GooglePlayGames.BasicApi;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour
{

    void Start()
    {
      PlayGamesPlatform.Instance.Authenticate(ProcessAuthentication);
    }


    void ProcessAuthentication(SignInStatus status)
    {
      if (status == SignInStatus.Success)
      {
        GetAccessCode();
        // Continue with Play Games Services
      }
      else
      {
        // Disable your integration with Play Games Services or show a login button
        // to ask users to sign-in. Clicking it should call
        // PlayGamesPlatform.Instance.ManuallyAuthenticate(ProcessAuthentication).
      }
    }

    public void GetAccessCode()
    {
        PlayGamesPlatform.Instance.RequestServerSideAccess(
          /* forceRefreshToken= */ false,
          async code => {
              Debug.Log("구글 인증 코드 : " + code);

              BackndAuth.Instance.GetGPGS2AccessToken(code, googleCallback =>
              {
                  Debug.Log("GetGPGS2AccessToken 함수 호출 결과 " + googleCallback);

                  string accessToken = "";

                  if (googleCallback.IsSuccess())
                  {
                      accessToken = googleCallback.GetAccessToken();
                  }

                  BackndAuth.Instance.SignInWithProvider(accessToken, AuthProvider.GPGS2, callback =>
                  {
                      Debug.Log("뒤끝 로그인 성공했습니다. " + callback);
                  });
              });
          });
    }
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
message : Forbidden blocked user, 금지된 blocked user

---

## GPGS 로그인 에러 케이스

GPGS 로그인 실패 시 발생하는 에러는 GPGS github를 참고 부탁드리겠습니다.

- GPGS 깃허브 저장소 : https://github.com/playgameservices/play-games-plugin-for-unity

### GPGS 로그인 시도 시, 앱이 꺼지는 경우

> Application ID(1040339826718) must be a numeric value. Please verify that your manifest refers to the correct project ID.

위와 같은 에러가 발생하는 경우, Assets > GooglePlayGames > Plugins > Android > GooglePlayGamesManifest > AndroidManifest.xml의
**meta-data android:name="com.google.android.gms.games.APP_ID"**에 해당하는 value 값의 `\`를 `\u003`으로 변경해 주시기 바랍니다.
