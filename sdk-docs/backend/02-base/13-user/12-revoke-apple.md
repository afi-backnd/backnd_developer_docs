---
sidebar_label: "애플 토큰 만료(only iOS)"
description: "RevokeAppleToken"
---

# RevokeAppleToken
public BackendReturnObject **RevokeAppleToken**(string **authorizationCode**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| authorizationCode | string  | 애플 서버에 전송할 애플의 유저 인증 값 |

## 설명

:::danger 애플 정책
[애플 심사 정책](https://developer.apple.com/kr/news/?id=12m75xbj)에 따라 Sign in with Apple 소셜 로그인이 구현되어있는 앱의 경우, 회원 탈퇴 시, Apple 서버에 해당 유저에 대한 토큰 취소 REST API를 호출해야합니다.
:::

애플 서버에 토큰 취소를 요청합니다. 해당 함수는 뒤끝에 로그인 되어 있는 상태에서 호출되어야합니다.  
따라서 회원탈퇴를 할 시에는 애플 토큰 만료 이후에 뒤끝 회원탈퇴를 호출해주시기 바랍니다.

서버에 요청을 보내는 authorizationCode는 뒤끝에서 페데레이션 로그인 시 필요한 id 토큰과는 다르며, 다음과 같은 값을 가지고 있습니다.  
또한 해당 값의 유효기간은 10분입니다.

```
cb3235099be9244a1b3c12b7c71e4afe9.0.srqyr.eOzlqRF-gg9UZvHXBwmGaA
```


## 사전 작업

해당 함수를 호출하기 위해서는 `뒤끝 콘솔 > 인증 정보 > 스토어 인증 정보`에 다음과 같은 입력칸에 올바른 값을 입력해야합니다.
* iOS Bundle Identifier
* Apple 개발자 계정의 Team ID
* Apple 프로젝트의 Key 이름
* Apple 프로젝트 Key(p8 파일)

![](/img/docs/guide/base/user/revoke-apple/console.png)


### iOS Bundle Identifier

[애플 개발자 계정](https://appstoreconnect.apple.com/apps) 접속 > 등록한 앱 클릭 > 앱 정보 > 일반 정보 > 번들 ID

![](/img/docs/guide/base/user/revoke-apple/identifier.png)

### Apple 개발자 계정의 Team ID

[애플 개발자 계정](https://appstoreconnect.apple.com/apps) 접속 > 우측 상단 아이디 클릭 > 프로필 편집 클릭

![](/img/docs/guide/base/user/revoke-apple/team-id.png)

### Apple 프로젝트 Key

1. [Certificates, Identifiers & Profiles](https://developer.apple.com/account/resources/authkeys/list)에 들어가 Key를 추가합니다.

![](/img/docs/guide/base/user/revoke-apple/create-key.png)

2. Sign In With Apple에 체크, Configure를 클릭합니다.

![](/img/docs/guide/base/user/revoke-apple/siwa-configure.png)

3. 애플 로그인을 구현한 앱을 선택합니다.

![](/img/docs/guide/base/user/revoke-apple/register.png)

4. Save를 클릭합니다.

![](/img/docs/guide/base/user/revoke-apple/save.png)

5. Download를 클릭하여 p8 파일을 다운받습니다.

![](/img/docs/guide/base/user/revoke-apple/key-name.png)

### Apple 프로젝트의 Key 이름

Apple 프로젝트의 Key를 다운받았던 곳에서 key 이름을 복사합니다.

![](/img/docs/guide/base/user/revoke-apple/key-name.png)

## Example
해당 예제는 [Lupidan/apple-signin-unity](https://github.com/lupidan/apple-signin-unity)의 기능을 참고하여 authorizationCode를 발급하고 있습니다.

### 동기
```js
//즉시 탈퇴
public void Login()
    {
        // If the current platform is supported
        if (AppleAuthManager.IsCurrentPlatformSupported)
        {
            // Creates a default JSON deserializer, to transform JSON Native responses to C# instances
            var deserializer = new PayloadDeserializer();
            // Creates an Apple Authentication manager with the deserializer
            this.appleAuthManager = new AppleAuthManager(deserializer);
        }

        var loginArgs = new AppleAuthLoginArgs(LoginOptions.IncludeEmail | LoginOptions.IncludeFullName);

        this.appleAuthManager.LoginWithAppleId(
            loginArgs,
            credential =>
            {
                // Obtained credential, cast it to IAppleIDCredential
                var appleIdCredential = credential as IAppleIDCredential;
                if (appleIdCredential != null)
                {
                    // Apple User ID
                    // You should save the user ID somewhere in the device
                    var userId = appleIdCredential.User;

                    Debug.Log("userId : " + userId);
                    // Email (Received ONLY in the first login)
                    var email = appleIdCredential.Email;

                    Debug.Log("email : " + email);
                    // Full name (Received ONLY in the first login)
                    var fullName = appleIdCredential.FullName;

                    Debug.Log("fullName : " + fullName);

                    // Identity token
                    var identityToken = Encoding.UTF8.GetString(
                        appleIdCredential.IdentityToken,
                        0,
                        appleIdCredential.IdentityToken.Length);

                    Debug.Log("identityToken : " + identityToken);


                    // Authorization code
                    var authorizationCode = Encoding.UTF8.GetString(
                        appleIdCredential.AuthorizationCode,
                        0,
                        appleIdCredential.AuthorizationCode.Length);

                    Debug.Log("authorizationCode : " + authorizationCode);
                    // And now you have all the information to create/login a user in your system

                    var bro = Backend.BMember.RevokeAppleToken(authorizationCode);
                    Debug.Log("RevokeAppleToken : " + bro);

                    if(bro.IsSuccess()) 
                    {
                        var bro2 = Backend.BMember.WithdrawAccount();
                        
                        Debug.Log("회원탈퇴 : " + bro2);
                    }
                }
            },
            error =>
            {
                // Something went wrong
                var authorizationErrorCode = error.GetAuthorizationErrorCode();
                Debug.LogError(authorizationErrorCode.ToString());
            });
    }
```

### 비동기
```js
//즉시 탈퇴
string authorizationCode = "cb3235099be9244a1b3c12b7c71e4afe9.0.srqyr.eOzlqRF-gg9UZvHXBwmGaA";


Backend.BMember.RevokeAppleToken(authorizationCode, callback  => 
{
    if(callback.IsSuccess()) 
    {
      Backend.BMember.WithdrawAccount(callback2 => 
      {
        if(callback2.IsSuccess()) 
        {
          Debug.Log("회원 탈퇴에 성공했습니다.");
        }
      });
    }
});
```

### SendQueue
```js
string authorizationCode = "cb3235099be9244a1b3c12b7c71e4afe9.0.srqyr.eOzlqRF-gg9UZvHXBwmGaA";

//즉시 탈퇴
SendQueue.Enqueue(Backend.BMember.RevokeAppleToken, authorizationCode, callback => 
{
    if(callback.IsSuccess()) 
    {
      SendQueue.Enqueue(Backend.BMember.WithdrawAccount,callback2 => 
      {
        if(callback2.IsSuccess()) 
        {
          Debug.Log("회원 탈퇴에 성공했습니다.");
        }
      });
    }
});
```

## ReturnCase

### Success cases
**회원 탈퇴에 성공한 경우**  
statusCode : 204  
message : Success
