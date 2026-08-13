---
description: "뒤끝 애플 로그인 SDK"
---

# 뒤끝 애플 로그인 SDK

:::info ios 미지원 안내
뒤끝 애플 로그인 SDK는 현재 Android 버전만 지원하고 있습니다.
:::

뒤끝 애플 로그인 SDK는 Andorid에서 웹뷰를 통해 애플 로그인 기능을 사용하도록 뒤끝에서 가공하여 제공하는 SDK입니다.

애플 [Certificates, Identifiers & Profiles 페이지](https://developer.apple.com/account/resources/identifiers/list/serviceId)에서 리다이렉트 URL을 입력하고,
SDK에서 간단한 핸들러를 등록하여 로그인 함수를 호출하는 것으로 쉽게 로그인이 가능해집니다.

:::caution progard user안내
만약 프로가드를 사용중일 경우, 아래와 같은 예외 처리가 필요합니다.  
```
-keep class io.thebackend.applelogin.** {*;}
```
:::


## 애플 로그인 SDK 적용  
[BackendAppleLogin-1.2.0.unitypackage](https://developer.thebackend.io/sdk/apple-login/BackendAppleLogin-1.2.0.unitypackage) \[2024-05-28]  

애플 로그인 SDK는 **Android만 지원**하고 있으며, 유니티 에디터에서는 작동하지 않습니다.  

## 애플 Services Ids 설정 
애플 로그인을 사용하기 위해서는 [Certificates, Identifiers & Profiles 페이지](https://developer.apple.com/account/resources/identifiers/list/serviceId)에서 애플 로그인용 Service IDs를 생성해야합니다.

### 1. Services IDs 생성
[Certificates, Identifiers & Profiles 페이지](https://developer.apple.com/account/resources/identifiers/list/serviceId) > Identifiers탭에서 우측 상단에 태그를 클릭하여 Services IDs를 선택합니다.

![](/img/docs/guide/toolkit/apple-login/select-services-id.png)

Identifiers 우측에 + 버튼을 눌러 새로운 Identieifers를 생성합니다.
![](/img/docs/guide/toolkit/apple-login/select-plus.png)

Services Ids를 선택합니다.
![](/img/docs/guide/toolkit/apple-login/select-drop-box.png)

Identifiers를 입력합니다.  
해당 값은 고유한 값이어야 합니다.  
추가적으로 해당 Identifiers를 설명할 Description도 입력합니다.  
Continue를 눌러 Identifiers 생성을 완료합니다.  

![](/img/docs/guide/toolkit/apple-login/input-identifier.png)


### 2. Sign in with Apple 연결
생성된 Identifiers를 클릭합니다.
![](/img/docs/guide/toolkit/apple-login/created-id.png)

아래 애플 기능 중 Sign in with Apple를 체크하고, Configure 버튼을 클릭합니다.
![](/img/docs/guide/toolkit/apple-login/select-siwa.png)

애플 로그인을 연결할 App ID를 선택하고, Register Website URLS에는 다음과 같이 입력합니다.

* Domains and Subdomains : **redirect.thebackend.io**
* Return URLs : **https://redirect.thebackend.io**

![](/img/docs/guide/toolkit/apple-login/input-redirect.png)

입력을 완료하였다면 Save를 클릭합니다.
![](/img/docs/guide/toolkit/apple-login/save-siwa.png)



### 뒤끝 콘솔 입력

생성한 Identifiers를 복사합니다.
![](/img/docs/guide/toolkit/apple-login/copy-identifiers.png)


뒤끝 콘솔에 인증정보 > 스토어 인증 정보에서 iOS Bundle Identifier for Web Login에 입력합니다.

![](/img/docs/guide/toolkit/apple-login/backend-console-input.png)
