---
sidebar_label: "구글 클라우드 플랫폼 설정"
description: "구글 클라우드 플랫폼 설정"
---

# 구글 클라우드 플랫폼 설정  
Android로 구글 로그인을 사용하려면 구글 콘솔에 앱이 등록되어 있어야합니다.  
앱을 등록하셨을 경우 [구글 클라우드 플랫폼에 프로젝트 생성](#2-구글-클라우드-플랫폼에-프로젝트-생성) 부터 시작하시면 됩니다.  

## 1. 구글 플레이에 어플리케이션 등록  
[구글 플레이 콘솔](https://play.google.com/apps/publish/) > 앱 만들기 > 출시 > 프로덕션 or (공개, 비공개, 내부) 테스트 > 새 버전 만들기  

* 1\) Google Play 앱 서명에서 **계속**을 눌러 앱 서명을 등록하여 **앱 서명 키 인증서**를 발급해 줍니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/google-1.png" />
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/google-2.png" />

* 2\) APK 또는 App Bundle을 업로드하여 **업로드 키 인증서**까지 발급해 줍니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/google-3.png" />

* 3\) 발급된 앱 서명은 왼쪽 메뉴 중 **설정 > 앱 서명**에서 확인할 수 있으며, 이후 구글 클라우드 플랫폼에 등록할 때 필요합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/google-4.png" />

## 2. 구글 클라우드 플랫폼에 프로젝트 생성
* 1\) [구글 클라우드 플랫폼](https://console.cloud.google.com/) > 프로젝트 선택 > 새 프로젝트 만들기
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/cloud-1.png" />

* 2\) 원하는 이름의 프로젝트 이름을 입력하고 만들기를 클릭합니다.
* 3\) 해당 프로젝트 선택 후 사이드 바에서 API 및 서비스 > OAuth 동의 화면을 클릭합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/cloud-2.png" />

* 4\) User Type을 외부로 클릭합니다.
* 5\) 필수항목인 앱 이름, 사용자 지원 이메일, 개발자 연락처 정보만 입력 후 저장 후 계속을 클릭합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/cloud-3.png" />

* 6\) 범위항목에서는 따로 추가 및 수정할 정보 없이 저장 후 계속을 클릭합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/cloud-4.png" />

* 7\) 구글로 로그인 기능을 테스트할 계정을 추가합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/cloud-5.png" />


## 3. Android 인증용 ClientID 생성
Android에서 구글로 로그인 기능을 이용하려면 다음과 같이 Client ID가 총 3개 생성되어야 합니다.
* 앱 서명 키로 만들어진 Android Client ID
* 업로드 키로 만들어진 Android Client ID
* SDK내에서 서버로 요청을 보낼 Web Client ID

### 3-1. 앱 서명 키로 만들어진 Android Client ID 생성
1) 사용자 인증 정보 만들기 - OAuth 클라이언트 ID를 클릭합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/client-1.png" />

2) 애플리케이션 유형을 Android로 선택하고 구글 콘솔에 업로드된 연결하고자 하는 앱의 packageName을 입력합니다.

3) 구글 콘솔 - 해당 앱 - 설정 - 앱 무결성에 있는 앱 서명 키 인증서 항목의 SHA-1 인증서 지문을 복사하여 SHA-1 인증서 디지털 지문에 붙여넣습니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/android/sha.png" />

4) 만들기를 클릭합니다.

### 3-2. 업로드 키로 만들어진 Android Client ID 생성

1) 사용자 인증 정보 만들기 - OAuth 클라이언트 ID를 클릭합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/client-1.png" />

2) 애플리케이션 유형을 Android로 선택하고 구글 콘솔에 업로드된 연결하고자 하는 앱의 packageName을 입력합니다.

3) 구글 콘솔 - 해당 앱 - 설정 - 앱 무결성에 있는 업로드 키 인증서 항목의 SHA-1 인증서 지문을 복사하여 SHA-1 인증서 디지털 지문에 붙여넣습니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/android/upload.png" />

4) 만들기를 클릭합니다.

### 3-3.  유니티에서 사용할 Web Client ID 생성

1) 사용자 인증 정보 만들기 - OAuth 클라이언트 ID를 클릭합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/client-1.png" />

2) 애플리케이션 유형을 웹 애플리케이션으로 선택합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/android/web.png" />

3) 만들기를 클릭합니다.

## 4. Web용 Client ID 복사
3-3 항목에서 생성한 웹 클라이언트의 클라이언트 ID를 복사합니다.
해당  값은 [Android 구글 로그인 코드](/sdk-docs/backend/toolkit/google-login/android/code)에서 BackendFederation.Android.GoogleLoign()의 **webClientId** 인자값으로 사용됩니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/android/web-copy.png" />

## 뒤끝 ToolKit 인스펙터를 통해 설정

뒤끝 구글 로그인 SDK를 설치한 이후, 유니티 상단 표시줄 > TheBackend > ToolKit > GoogleLogin Settings를 클릭하여 Android 칸에서 webClientId를 입력할 수 있습니다.
![](/img/docs/guide/toolkit/google-login/inspector-web.png)
