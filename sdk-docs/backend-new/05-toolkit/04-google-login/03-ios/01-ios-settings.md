# iOS용 구글 클라우드 플랫폼 설정  

iOS로 구글 로그인을 이용하려면 구글 클라우드 플랫폼(GCP) 프로젝트에 iOS용 클라이언트 ID가 존재해야 합니다.  
이미 안드로이드용 구글 로그인을 위해 생성한 GCP 프로젝트가 존재한다면 해당 프로젝트를 이용하여 [iOS 인증용 ClientID 생성](#3-ios용-client-id-생성) 항목부터 시작하면 됩니다.  

## 1. 구글 클라우드 플랫폼에 프로젝트 생성  
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


## 2. App Store Connect에서 정보 가져오기  
iOS에서 구글로 로그인 기능을 이용하려면 다음과 같이 Client ID가 총 1개 생성되어야 합니다.  
* iOS 인증용 iOS Client ID  

해당 클라이언트 ID를 생성하기 위해서는 다음과 같은 정보가 필요하며 AppStoreConnect 콘솔에서 얻으실 수 있습니다  
* App Store Connect에 등록된 Bundle ID  
* App Store Connect에 등록된 Apple ID  
* App Store Connect에 등록한 Team ID  

### 2-1. Bundle ID 가져오기  
App Store Connect > 해당 앱 > 일반 정보 > 앱 정보 > **번들 ID**를 복사합니다.  
 > <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/appstore.png" />

### 2-2. Apple ID 가져오기  
App Store Connect > 해당 앱 > 일반 정보 > 앱 정보 > **Apple ID**를 복사합니다.  
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/bundle.png" />

### 2-3. Team ID 가져오기  
App Store Connect 상단에 사용자 및 액세스 > 관리자 계정 선택 > People >  **팀 ID**를 복사합니다.  
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/team.png" />

## 3. iOS용 Client ID 생성  
1) 사용자 인증 정보 만들기 - OAuth 클라이언트 ID를 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/client-1.png" />

2) 애플리케이션 유형을 iOS로 선택하고 위 항목에서 복사한 정보들을 차례대로 입력합니다.  
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/client.png" />

3) 만들기를 클릭합니다.  


## 4. SDK 내에서 사용할 iOS Client 설정

### 4-1. 클라이언트 ID 설정
생성한 iOS 클라이언트의 **클라이언트 ID**를 복사합니다.  

뒤끝 구글 로그인 SDK를 설치한 이후, 유니티 상단 표시줄 > TheBackend > ToolKit > GoogleLogin Settings를 클릭하여 IOS 칸에서 Ios Client ID를 입력할 수 있습니다.
![](/img/docs/guide/toolkit/google-login/inspector-ios.png)

### 4-2. iOS URL 스키마 설정
생성한 iOS 클라이언트의 **iOS URL 스키마**를 복사합니다.
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/urlschema.png" />


뒤끝 구글 로그인 SDK를 설치한 이후, 유니티 상단 표시줄 > TheBackend > ToolKit > GoogleLogin Settings를 클릭하여 IOS 칸에서 Ios Url Schema를 입력할 수 있습니다.
![](/img/docs/guide/toolkit/google-login/inspector-url.png)
