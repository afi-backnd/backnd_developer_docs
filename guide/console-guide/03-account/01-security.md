import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 보안 설정

## 2단계 인증
뒤끝 상단(GNB)의 `드롭다운 메뉴 → 환경 설정` 메뉴에서 2단계 인증을 설정할 수 있습니다.  

<ConsoleLinkButton text="환경 설정 바로가기" href="https://console.thebackend.io/account/setting" feature="환경 설정" title="보안 설정" />

![](/img/docs/guide/base/security/dropdown.png)

## 2단계 인증 사용
2단계 인증을 사용하기 위해서는 OTP 인증 앱이 필요합니다. OTP 인증 앱이 없는 경우 다음 OTP 인증 앱 중 하나를 설치해 주세요.  
- [Google OTP](https://support.google.com/accounts/answer/1066447)
- [Microsoft Authenticator](https://www.microsoft.com/ko-kr/security/mobile-authenticator-app)
- [Authy](https://authy.com/features/)

설치된 OTP 인증 앱에서 다음 QR 코드를 스캔한 후 생성된 인증 번호를 입력해 주시면 설정이 완료됩니다.  
><img src="https://developer.thebackend.io/static/img/newconsole/gm/보안설정1.png" />

## 2단계 인증 로그인
2단계 인증을 사용 중인 계정은 로그인 화면에서 인증 번호 6자리를 입력해야 로그인이 가능합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/gm/2단계인증로그인.png" />

## 2단계 인증 해제
설정한 2단계 인증을 해제하기 위해서는 비밀번호가 필요합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/gm/보안설정2.png" />
