---
sidebar_label: "Facebook 인증 예제"
sidebar_position: "4.5"
description: "Facebook SDK 사용"
---

# Facebook SDK 사용

:::info Flow of the Process

1. Facebook 을 통해 로그인
2. 페이스북 Access token 획득
3. 이 페이스북 Access token 검증을 통해 뒤끝 Access token 획득
4. 뒤끝 Access token을 통해 뒤끝 서버에 로그인
5. 페이스북 액세스 토큰을 이용해 Backend.BMember.AuthorizeFederation("accessToken", FederationType.FaceBook) 호출

:::

## 초기 설정
### 1. 페이스북 앱 정보와 뒤끝 서버 연동
* 페이스북 개발자 페이지에서 앱 정보를 등록합니다.  
[페이스북 개발자 페이지](https://developers.facebook.com/) > 오른쪽 상단 시작하기  
* 앱 ID 와 앱 시크릿 코드를 가져와 뒤끝콘솔(서버 설정 > 인증 정보)에 등록합니다.  

### 2. Unity Project와 페이스북 앱 정보 연동
* Facebook SDK를 다운로드해 Unity Project에 import  
[Facebook SDK for Unity](https://developers.facebook.com/docs/unity/downloads)  
* Unity Project에 페이스북 App ID 등록  
상위 메뉴 Facebook > Edit Settings > App ID 등록  
* 페이스북 페이지에 앱 관련 정보 등록  
Unity 상위 메뉴 Facebook > Edit Settings에 나온 **Package Name, Class Name, Debug Android Key Hash**를 **[페이스북 개발자 페이지](https://developers.facebook.com/apps/) > 해당 앱 > 설정 > 기본 설정 > 플랫폼 추가 > Android**선택 후 등록합니다.  

> **ID 토큰을 받기 위한 설정은 모두 끝났습니다.**  
 페이스북 앱 설정에 등록한 페이스북 아이디를 통해 ID token을 받을 수 있으며,  
 추가적으로 페이스북 개발 콘솔 > 해당 앱 > 설정 > 역할 > 테스터 > 테스터 추가 메뉴를 통해 테스터를 추가할 수 있습니다.  
자세한 설명은 [페이스북 테스트 유저](https://developers.facebook.com/docs/development/build-and-test/test-users) 문서를 참고해 주세요.  


### 3. 페이스북 콘솔 액세스 레벨 설정

페이스북 페더레이션 로그인을 위해서는 해당 public_profile 접근 권한과 email 접근 권한이 필요합니다.  
아래 페이스북 개발자 문서를 참고하여 **public_profile 및 email의 고급 액세스 권한**으로 업그레이드해야 합니다.  
* [페이스북 액세스 레벨](https://developers.facebook.com/docs/graph-api/overview/access-levels/)  


 ### 4. 페이스북 콘솔 모드 설정

 페이스북 콘솔에서 해당 앱의 모드를 변경할 수 있습니다.  
 각 모드 별로 아래의 차이점이 존재합니다.  

| 모드 구분 | 테스트 계정 및 개발 계정 | 일반 계정 |
| --- | --- | --- |
| 개발 모드 | 액세스레벨 관계없이 뒤끝 페더레이션 인증이 가능합니다. | 고급 액세스 레벨 권한이 없으면 뒤끝 페더레이션 인증은 불가능합니다. |
| 라이브 모드 | 고급 액세스 레벨 권한이 없으면 뒤끝 페더레이션 인증이 불가능합니다. | 고급 액세스 레벨 권한이 없으면 뒤끝 페더레이션 인증이 불가능합니다. |
> 모드 구분 없이 페이스북 SDK를 통해 개발 계정, 일반 계정, 테스트 계정 모두 페이스북 계정 로그인은 가능하나, 
**고급 액세스 레벨 권한이 없으면 뒤끝 서버에서 페이스북 서버와 인증하는 과정에서 테스트 및 개발 계정을 제외하고는 인증이 실패하기 때문에 페더레이션 인증이 불가능**합니다.

페이스북의 모드 구분에 대해서는 아래 페이스북 개발자 문서를 참고해 주세요.  
* [페이스북 앱 모드](https://developers.facebook.com/docs/development/build-and-test/app-modes)




----------------------------------------------------------------------------------------------------

# AuthorizeFederation
public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**);
public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**, string **ect**);

## 파라미터

| Value  | Type  |  Description |
| --- | --- | --- |
| federationToken | string | 각 로그인 플러그인을 통해 생성된 token 값 |
| federationType | FederationType | 페더레이션의 종류.(FederationType.FaceBook) |
| ect| string | (Optional) 부가적으로 나오는 정보들 중에 저장하고 싶은 정보|

## 설명
페이스북의 회원정보 토큰 값을 이용하여 회원가입/로그인을 시도합니다.

페이스북 로그인 진행 후, ID 토큰을 얻어 뒤끝서버를 통해 게임에 가입할 수 있습니다. 페이스북 권한 설정에 `var perms = new List<string>() { "email" }`이 포함되면, **뒤끝 콘솔 > 유저 관리**에서 회원의 이메일 정보를 알 수 있습니다.

## Example
```js
// Include Facebook namespace
using Facebook.Unity;

// Awake function from Unity's MonoBehavior
void Awake ()
{
    if (!FB.IsInitialized) {
        // Initialize the Facebook SDK
        FB.Init(InitCallback, OnHideUnity);
    } else {
        // Already initialized, signal an app activation App Event
        FB.ActivateApp();
    }
}

private void InitCallback ()
{
    if (FB.IsInitialized) {
        // Signal an app activation App Event
        FB.ActivateApp();
        // Continue with Facebook SDK
        // ...
    } else {
        Debug.Log("Failed to Initialize the Facebook SDK");
    }
}

private void OnHideUnity ( bool isGameShown )
{
    if (!isGameShown) {
        // Pause the game - we will need to hide
        Time.timeScale = 0;
    } else {
        // Resume the game - we're getting focus again
        Time.timeScale = 1;
    }
}

public void facebookLogin()
{
    // 읽어올 권한을 설정
    // 뒤끝 콘솔 > 유저 관리에서 회원의 이메일 정보를 알 수 있습니다.
    var perms = new List<string>() { "public_profile", "email" };
    FB.LogInWithReadPermissions(perms, AuthCallback);
}

private void AuthCallback ( ILoginResult result )
{
    // 로그인 성공
    if (FB.IsLoggedIn) {
        // AccessToken class will have session details
        var aToken = Facebook.Unity.AccessToken.CurrentAccessToken;
        // Print current access token's User IDDebug.Log(aToken);
        string facebookToken = aToken.TokenString;
        // 뒤끝 서버에 획득한 페이스북 토큰으로 가입 요청
        Backend.BMember.AuthorizeFederation(facebookToken, FederationType.Facebook);

    } else {
        Debug.Log("User cancelled login");
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
message : forbidden blocked user, 금지된 blocked user 

**뒤끝 콘솔에 입력한 페이스북 정보가 잘못되었을 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad bad facebook app info, 잘못된 bad facebook app info 입니다
