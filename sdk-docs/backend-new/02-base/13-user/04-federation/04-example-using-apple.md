---
sidebar_label: Apple 로그인 인증 예제
description: "Apple 로그인 인증 예제"
---

# Sign In with Apple(Apple로 로그인) 사용

:::info Flow of the Process

1. Apple로 로그인 기능을 통해 애플 계정으로 로그인
2. 애플 identity Token 획득
3. 애플 identity Token 검증을 통해 뒤끝 Access token 획득
4. 뒤끝 Access token을 통해 뒤끝 서버에 로그인
5. 액세스 토큰을 한번 발급받으면 이를 이용하여 토큰 로그인도 수행할 수 있습니다. 토큰에 대한 설명은 <a href="/sdk-docs/backend/base/user/token/accesstoken" target="_blank">해당 문서</a>를 참고해 주세요.

:::

:::danger 주의

- Apple로 로그인 기능은 iOS 13.0 이상에서만 정상적으로 작동합니다.
  - macOS 10.14(Mojave) 또는 10.15(Catalina) 및 iOS 13.0용 Xcode 11이 필요합니다.
  - 또한 개발 중인 애플리케이션에 이 기능을 사용하려면 Apple 개발자 포털에서 Apple로 로그인 기능을 먼저 활성화해야 합니다.
:::  

:::danger <b>Apple 개발자 계정 간 게임 이관 시 주의사항</b>

최초 운영하던 계정에서 타 계정으로 게임을 이관하는 경우 Team ID가 변경되면서 유저들이 로그인할 때의 유니크한 값(유저별 sku)이 변경되어 기존 정보로 로그인이 불가합니다.

이는 Apple의 정책으로 인한 부분으로 원활한 게임 서비스를 위해서는 뒤끝의 유료 기술 지원이 필요합니다.

이관을 진행하시고자 하는 경우 커뮤니티 혹은 help@backnd.com로 사전에 문의하시어 게임 운영에 불편 없으시기 바랍니다.  
:::

## 초기 설정

### Apple 개발자 포털에서 Apple로 로그인 기능 활성화

[Apple Developer - Member Center](https://developer.apple.com/account/)에 접속하여, **Certificates, Identifiers & Profiles**를 클릭하여 이동 후, Identifiers에 있는 해당 앱에 들어가 **Sign In with Apple**을 체크해 주세요.  
<img src="https://developer.thebackend.io/static/img/unity/apple/00.png" />

---

# SignInWithProvider

public Task&lt;RequestResult&gt; **SignInWithProviderAsync**(string **authToken**, AuthProvider **provider**);  
public Task&lt;RequestResult&gt; **SignInWithProviderAsync**(string **authToken**, AuthProvider **provider**, string **ect**);

## 파라미터

| Value           | Type           | Description                                                 |
| --------------- | -------------- | ----------------------------------------------------------- |
| authToken       | string         | 각 로그인 플러그인을 통해 생성된 token 값                   |
| provider        | AuthProvider   | 외부 로그인 제공자의 종류.(AuthProvider.Apple)                    |
| ect             | string         | (Optional) 부가적으로 나오는 정보들 중에 저장하고 싶은 정보 |

## 설명

애플의 회원정보 토큰 값을 이용하여 회원가입/로그인을 시도합니다.

## 일반 코드 Example

```js
var reqResult = await BackndAuth.Instance.SignInWithProviderAsync("idToken", AuthProvider.Apple, "siwa");
if (reqResult.IsSuccess())
{
    Debug.Log("APPLE 로그인 성공");
    //성공 처리
}
else
{
    Debug.LogError("Apple 로그인 실패");
    //실패 처리
}
```

## Lupidan 오픈소스 Example

해당 코드는 [Lupidan](https://github.com/lupidan/apple-signin-unity)의 예제 프로젝트의 GameMenuHandler.cs입니다.

> 해당 오픈소스에 대한 코드는 뒤끝에서 공식적으로 제공되는 코드가 아니므로 지원 및 문의는 불가능하다는점 참고해주세요.

```js
public async Task SetupAppleData(string appleUserId, ICredential receivedCredential)
{
  var appleIdCredential = receivedCredential as IAppleIDCredential;
  if(appleIdCredential.IdentityToken != null)
  {
    var identityToken = Encoding.UTF8.GetString(appleIdCredential.IdentityToken, 0, appleIdCredential.IdentityToken.Length);

    Debug.Log("토큰 : " + identityToken);

    var reqResult = await BackndAuth.Instance.SignInWithProviderAsync(identityToken, FederationType.Apple);
    Debug.Log("로그인 결과 : " + reqResult);
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

**(뒤끝 애플 로그인 SDK 이용 시) 입력한 identifier와 콘솔에서 설정한 identifier의 값이 올바르지 않는 경우**  
statusCode : 401  
errorCode : BadUnauthorizedException  
message : bad bad appId, 잘못된 bad appId입니다

---

## 빌드 후, Xcode에서 설정하기

1\) Build Phases > Link Binary With Libraries 추가 > AuthenticationServices.framework 추가 > Status를 Optional로 변경

<img src="https://developer.thebackend.io/static/img/unity/apple/01.png" />

<img src="https://developer.thebackend.io/static/img/unity/apple/02.png" />

2\) Signing & Capabilities > Capabilitiy 추가 > Sign In with Apple 추가
<img src="https://developer.thebackend.io/static/img/unity/apple/03.png" />
