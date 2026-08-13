---
description: "애플 로그인 유저 회원 탈퇴 웹 링크"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 애플 로그인 유저 회원 탈퇴 웹 링크

## 설명

```
Backend.BMember.AuthorizeFederation("", FederationType.Apple);
```

뒤끝에서 다음과 위와 함수로 애플 로그인을 진행한 유저가 회원 탈퇴 웹 링크를 통해 회원 탈퇴를 진행하려 할 경우, 다음과 같은 설정이 필요합니다.

1. Apple Developer > 로그인에 사용된 웹 클라이언트 아이디 > 승인된 리디렉션 URI에 https://auth0.thebackend.io/apple/token 입력
2. 뒤끝 콘솔 > 인증 정보 > iOS Bundle Identifier for Web Login

:::info 안드로이드용 웹 형식 애플 로그인을 구현한 경우
뒤끝 애플 로그인 SDK를 통해 애플 로그인을 구현한 경우, 해당 구현 절차에서 생성된 Services IDs의 Sign In With Apple 설정에 도메인과 Urls를 추가만 해주면 됩니다.(2-2 부터 진행)
:::

## 1. 애플 Services Ids 설정 
애플 로그인을 사용하기 위해서는 [Certificates, Identifiers & Profiles 페이지](https://developer.apple.com/account/resources/identifiers/list/serviceId)에서 애플 로그인용 Service IDs를 생성해야합니다.

### 1. Services IDs 생성
1) [Certificates, Identifiers & Profiles 페이지](https://developer.apple.com/account/resources/identifiers/list/serviceId) > Identifiers탭에서 우측 상단에 태그를 클릭하여 Services IDs를 선택
![](/img/docs/guide/toolkit/apple-login/select-services-id.png)

2) Identifiers 우측에 + 버튼을 눌러 새로운 Identieifers 생성
![](/img/docs/guide/toolkit/apple-login/select-plus.png)

3) Services Ids 선택
![](/img/docs/guide/toolkit/apple-login/select-drop-box.png)

4) 고유한 Identifiers를 입력. 추가적으로 해당 Identifiers를 설명할 Description 입력 후, Continue를 눌러 Identifiers 생성을 완료
![](/img/docs/guide/toolkit/apple-login/input-identifier.png)


### 2. Sign in with Apple 연결
1) 생성된 Identifiers 클릭.
![](/img/docs/guide/toolkit/apple-login/created-id.png)

2) 아래 애플 기능 중 Sign in with Apple를 체크하고, Configure 버튼을 클릭
![](/img/docs/guide/toolkit/apple-login/select-siwa.png)

3) 애플 로그인에 사용된 App ID를 선택하고, Register Website URLS에는 다음과 같이 입력(만약 안드로이드용 웹형식 애플 로그인을 구현하였다면 해당 Identifier에서 URLS 추가)
* Domains and Subdomains : **auth0.thebackend.io**
* Return URLs : **auth0.thebackend.io/apple/token**
![](/img/docs/guide/toolkit/apple-login/withdraw-url.png)

4) 입력 완료후, Save 버튼 클릭
![](/img/docs/guide/toolkit/apple-login/save-siwa.png)


### 3. 뒤끝 콘솔 입력

1) 생성한 Identifiers를 복사
![](/img/docs/guide/toolkit/apple-login/copy-identifiers.png)


2) 뒤끝 콘솔에 인증정보 > 스토어 인증 정보에서 iOS Bundle Identifier for Web Login에 입력

<ConsoleLinkButton text="인증 정보 바로가기" menu="settingAuth" feature="인증 정보" title="애플 로그인 유저 회원 탈퇴 웹 링크" />

![](/img/docs/guide/toolkit/apple-login/backend-console-input.png)
