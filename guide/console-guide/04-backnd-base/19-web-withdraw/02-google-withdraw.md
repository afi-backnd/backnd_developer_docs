import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 구글 로그인 유저 회원 탈퇴 웹 링크

## 설명

```
Backend.BMember.AuthorizeFederation("", FederationType.Google);
Backend.BMember.AuthorizeFederation("", FederationType.GPGS2);
```

뒤끝에서 다음과 위와 함수로 구글 로그인 혹은 GPGS 로그인을 진행한 유저가 회원 탈퇴 웹 링크를 통해 회원 탈퇴를 진행하려 할 경우, 다음과 같은 설정이 필요합니다.

:::info 구글 로그인, GPGS2 로그인 인증값 동일 안내
구글 로그인과 GPGS2 로그인 모두 동일한 절차를 통해 웹 탈퇴 링크를 생성합니다.
:::

1. 뒤끝 콘솔 > 인증 정보 > 구글 로그인 인증 정보 입력

<ConsoleLinkButton text="인증 정보 바로가기" menu="settingAuth" feature="인증 정보" title="구글 로그인 유저 회원 탈퇴 웹 링크" />

2. 구글 클라우드 플랫폼 > 로그인에 사용된 웹 클라이언트 아이디 > 승인된 리디렉션 URI에 https://auth0.thebackend.io/google/token 입력


### 1. 구글 로그인 인증 정보 입력
구글 로그인 인증 정보 입력은 [해당 문서](/guide/console-guide/server-setting/authenciation)를 참고해주세요.

### 2. 승인된 리디렉션 URI 입력
1\) [Google Cloud Platform](https://console.cloud.google.com/apis/credentials) > API 및 서비스 > 사용자 인증 정보 > 웹 클라이언트 선택

![](/img/docs/guide/console-google-info/select-webclient-id.png)


2\) 승인된 리디렉션 URI에 **https://auth0.thebackend.io/google/token**를 추가

![](/img/docs/guide/console-google-info/redirect-withdraw.png)

> https://auth0.thebackend.io는 gpgs v2 로그인 시 사용되는 uri로, 구글 로그인을 이용한 경우 해당 값은 입력하지 않아도 됩니다.  

만약 잘못된 구글 정보를 입력하였을 경우, 아래와 같이 웹 탈퇴 링크에서 invalid_client 에러가 발생합니다.

![](/img/docs/guide/base/web-withdraw/invalid_client.png)
