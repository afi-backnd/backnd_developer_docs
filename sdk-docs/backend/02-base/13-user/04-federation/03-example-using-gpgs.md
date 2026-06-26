---
sidebar_label: "[deprecated] GPGS V1 인증 예제"
---

# Google Play Game Service(GPGS) 사용

:::danger GPGS 및 Sign In with Google 이용 주의사항
**GPGS V1은 구글로부터의 지원이 종료되었습니다.  
GPGS V1을 사용해 오셨거나, 새롭게 사용하시고자 하는 경우 Sign In with Google을 사용해 주세요.**  
   
**GPGS V2 로그인 적용 시 Google Play Games 앱의 업데이트 상태나 캐시 데이터 문제로 인해 정상적으로 이용되지 않을 수 있습니다.  
이용자가 로그인 이상을 호소하는 경우, Google Play Games 앱이 최신 버전인지 확인하고 캐시 삭제 후 재시도하도록 안내해 주시기 바랍니다.  
또한 GPGS V2 로그인은 안드로이드 기기에서만 지원되므로, 보다 안정적인 연동을 위해 구글 로그인을 적용하실 것을 권장드립니다.**

GPGS V2 및 Sign In with Google은 다음과 같은 조건으로 이용이 가능합니다.

| 구분 | GPGS V2 | [deprecated] GPGS V1 | Sign In with Google |
|---|---|---|---|
| **로그인 정보** | PGS games_lite 정보 이용 | 구글 계정 정보 이용 | 구글 계정 정보 이용 |
| **계정간 호환** | GPGS V2 단독 <br /> GPGS V1 및 Sign In with Google 호환 불가 | Sign In with Google 호환 <br /> GPGS V2 호환 불가 | GPGS V1 호환 <br /> GPGS V2 호환 불가 |
| **iOS 지원** | 미지원 | 미지원 | 지원 |
:::

:::info Flow of the Process

1. Google Play Game Service를 통해 구글 계정으로 로그인
2. 구글 Access token 획득
3. 이 구글 Access token 검증을 통해 뒤끝 Access token 획득
4. 뒤끝 Access token을 통해 뒤끝 서버에 로그인
5. 액세스 토큰을 한번 발급받으면 이용하여 토큰 로그인도 수행할 수 있습니다. 토큰에 대한 설명은 <a href="/sdk-docs/backend/base/user/token/accesstoken" target="_blank">해당 문서</a>를 참고해 주세요.

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

3. 테스터용 이메일을 추가합니다
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Auth-4.PNG" />

4. 동의 화면 구성 후, 왼쪽 메뉴에 사용자 인증 정보를 클릭하여 **사용자 인증 정보**를 구성합니다.  
   **사용자 인증 정보 만들기 > OAuth 클라이언트 ID > Android 선택**  
   이 과정을 1번(사진 4)에서 등록한 어플리케이션의 앱 서명 > **앱 서명 인증서와 업로드 인증서 두 가지**를 모두 해줍니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/KEY-1.PNG" />
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/KEY-2.PNG" />

   다음과 같이 Google Cloud Platform에 **총 2개의 OAuth 2.0 클라이언트 ID**가 등록되어 있어야 합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/KEY-3.PNG" />

5. Play 게임 서비스에 사용자 인증 정보 추가
   4번에 Google Cloud Platform에서 추가한 2개의 Android 사용자 인증 정보를 추가합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Setting-1.PNG" />
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Setting-2.PNG" />

   다음과 같이 구글 콘솔에 2개의 사용자 인증정보가 추가되어야 합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/Setting-3.PNG" />

### 3. 테스터 설정

GPGS를 테스트하기 위해서는 **Google Cloud Platform과 구글 콘솔에 테스터가 등록**되어 있어야 합니다.

1. Google Cloud Platform에 테스터 추가
   해당 Google Cloud Platform 왼쪽 메뉴에 **OAuth 동의 화면 > 테스트 사용자 > +ADD USERS**를 클릭하여 테스터를 등록합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/TESTER-1.PNG" />

2. 구글 콘솔 - Play 게임 서비스에 테스터 추가
   왼쪽 메뉴 **Play 게임 서비스 > 테스터 > 출시 트랙**에서 **트랙 추가**를 클릭하여 앱을 게시한 트랙을 추가합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/TESTER-2.PNG" />

   이후, 테스터 항목에서 **테스터 추가**를 클릭하여 테스터를 추가합니다.(Google Cloud Platform에서 추가한 테스터와 동일해야 합니다.)
   <img src="https://developer.thebackend.io/static/img/unity/gpgs/TESTER-3.PNG" />

### 4. 유니티 설정

1. Unity Project - Google Play Games Services Plugin 추가  
    <https://github.com/playgameservices/play-games-plugin-for-unity>

   > <b>GPGS 버전 10 다운로드</b> > \[current-build] 폴더 > unitypackage import

   :::danger 주의!!!
   **GPGS 버전 11.01 이상은 GPGS V2을 이용해야 합니다.**  
   GPGS 버전 10 이하 버전 이용시에만 GPGS V1을 이용해 주세요.
   :::

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

   **ID 토큰을 받기 위한 설정은 모두 끝났습니다.**  
   구글 플레이 콘솔에 등록한 아이디를 통해 id token을 받을 수 있으며,  
   추가적으로 다른 아이디를 테스트하기 위해서는 **#3. 테스터 설정** 방법을 통해 테스터를 추가할 수 있습니다.

---

# AuthorizeFederation

public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**);  
public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**, string **ect**);

## 파라미터

| Value           | Type           | Description                                                 |
| --------------- | -------------- | ----------------------------------------------------------- |
| federationToken | string         | 각 로그인 플러그인을 통해 생성된 token 값                   |
| federationType  | FederationType | 페더레이션의 종류.(FederationType.Google)                   |
| ect             | string         | (Optional) 부가적으로 나오는 정보들 중에 저장하고 싶은 정보 |

## 설명

구글의 회원정보 토큰 값을 이용하여 회원가입/로그인을 시도합니다.

Google Play Game Services(GPGS)의 ID token 을 받기 위한 PlayGamesClientConfiguration의 default 설정을 custom 설정으로 변경 후, GPGS를 실행합니다.

> 해당 구글 계정이 게임에 첫 로그인(회원가입) 시 구글 token의 값이 null로 반환될 수 있습니다.  
> 다시 요청할 경우 정상적인 token값을 받을 수 있습니다.

## Example

```js
// GPGS 로그인
void Start(){
    // GPGS 플러그인 설정
    PlayGamesClientConfiguration config = new PlayGamesClientConfiguration
        .Builder()
        .RequestServerAuthCode(false)
        .RequestEmail() // 이메일 권한을 얻고 싶지 않다면 해당 줄(RequestEmail)을 지워주세요.
        .RequestIdToken()
        .Build();
    //커스텀 된 정보로 GPGS 초기화
    PlayGamesPlatform.InitializeInstance(config);
    PlayGamesPlatform.DebugLogEnabled = true; // 디버그 로그를 보고 싶지 않다면 false로 바꿔주세요.
    //GPGS 시작.
    PlayGamesPlatform.Activate();
}

public void GPGSLogin()
{
    // 이미 로그인 된 경우
    if(Social.localUser.authenticated == true)
    {
        BackendReturnObject BRO = Backend.BMember.AuthorizeFederation(GetTokens(), FederationType.Google, "gpgs");
    }
    else
    {
        Social.localUser.Authenticate((bool success) => {
            if(success){
                // 로그인 성공 -> 뒤끝 서버에 획득한 구글 토큰으로 가입 요청
                BackendReturnObject BRO = Backend.BMember.AuthorizeFederation(GetTokens(), FederationType.Google, "gpgs");
            }
            else
            {
                // 로그인 실패
                Debug.Log("Login failed for some reason");
            }
        });
    }
}

// 구글 토큰 받아옴
public string GetTokens()
{
    if(PlayGamesPlatform.Instance.localUser.authenticated)
    {
        // 유저 토큰 받기 첫 번째 방법
        string _IDtoken = PlayGamesPlatform.Instance.GetIdToken();
        // 두 번째 방법
        // string _IDtoken = ((PlayGamesLocalUser)Social.localUser).GetIdToken();
        return _IDtoken;
    }
    else
    {
        Debug.Log("접속되어 있지 않습니다. PlayGamesPlatform.Instance.localUser.authenticated :  fail");
        return null;
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

<!--
###  구글 로그인 시도 시, "Canceled"가 발생하는 경우
Canceled 발생하는 경우, GoogleSignInConfiguration의 WebClientId가 잘못된 경우일 수 있습니다.
파이어 베이스의 앱 설정에서 google-services.json을 다운로드해, oauth_client의 type이 3 인 값을 넣어 다시 한번 확인해 주세요.
> [파이어 베이스 콘솔](https://console.firebase.google.com) > 프로젝트 > 앱 설정 > **google-services.json**

```js
configuration = new GoogleSignInConfiguration
{
    RequestEmail = true,
    WebClientId = "google-service.json 파일의 oauth_client with type == 3 인 client_id 값을 복사하여 입력",
    RequestIdToken = true
};
```
-->

GPGS 로그인 실패 시 발생하는 에러는 GPGS github를 참고 부탁드리겠습니다.

- GPGS 깃허브 저장소 : https://github.com/playgameservices/play-games-plugin-for-unity

### GPGS 로그인 시도 시, 앱이 꺼지는 경우

> Application ID(1040339826718) must be a numeric value. Please verify that your manifest refers to the correct project ID.

위와 같은 에러가 발생하는 경우, Assets > GooglePlayGames > Plugins > Android > GooglePlayGamesManifest > AndroidManifest.xml의
**meta-data android:name="com.google.android.gms.games.APP_ID"**에 해당하는 value 값의 `\`를 `\u003`으로 변경해 주시기 바랍니다.
