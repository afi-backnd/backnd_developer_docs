---
description: "iOS 푸시 등록"
---

# iOS 푸시 등록

뒤끝 콘솔에서 보내는 푸시를 받는 기능을 제공합니다.  
* 다국어 지원
* 유저의 국가 코드가 존재하고, 콘솔에 해당 국가 푸시가 등록되어 있는 경우 → 해당 국가의 푸시를 발송합니다. 
* 유저의 국가 코드가 존재하지 않거나, 콘솔에 해당 국가 푸시가 등록되어 있지 않은 경우 → 기본 설정의 푸시를 발송합니다.  

:::info Flow of the Process
애플 개발자 계정 등록을 완료했다는 가정하에 진행합니다.  
1. 인증서 요청 생성, APP ID 생성
2. APNS 인증서 발급, 프로비저닝 생성
3. Xcode 프로젝트 설정하고, 빌드 하기
:::

iOS는 푸시 알림 발송을 위해서는 인증서가 필요합니다. 인증서를 발급받기 위한 첫 번째 과정으로 개인 인증서를 발급받아야 합니다.  

## 1. 인증서 요청(CertificateSigningRequest) 생성, APP ID 생성
### 1-1 인증서 요청(CertificateSigningRequest) 생성
애플 개발자 계정에서 인증서를 만들기 위해서는 키 체인을 이용해 **CertificateSigningRequest**를 만들어야 합니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/1-1.png" />

사용자 이메일 주소에는 Apple ID(개발자 계정)와 이름을 입력 후 아래와 같이 설정하여 **CertificateSigningRequest**를 생성합니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/1-2.png" />

  


><img src="https://developer.thebackend.io/static/img/unity/push_ios/1-3.png" />

### 1-2 APP ID 생성

푸시 알람을 받을 앱의 **APP ID**를 생성합니다. [Apple Developer - Member Center](https://developer.apple.com/account/)에 접속하여, Certificates, Identifiers & Profiles를 클릭하여 이동합니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/2-1_V2.png" />

**App IDs**를 클릭 후 **+버튼**을 눌러 새로운 앱을 추가해 줍니다

><img src="https://developer.thebackend.io/static/img/unity/push_ios/2-2_V2.png" />

**Bundle ID**는 Unity에서 설정한 값과 같아야 합니다. 아래 과정을 쭉 따라 진행합니다. 

><img src="https://developer.thebackend.io/static/img/unity/push_ios/2-3-1_V2.png" />
><img src="https://developer.thebackend.io/static/img/unity/push_ios/2-3-2_V2.png" />
><img src="https://developer.thebackend.io/static/img/unity/push_ios/2-3-3_V2.png" />


><img src="https://developer.thebackend.io/static/img/unity/push_ios/2-4_V2.png" />


## 2. APNS 인증서 발급, 프로비저닝 생성
### 2-1 APNS 인증서 발급
방금 만든 App이 App IDs 탭에 들어오면 보입니다. 이 App을 선택하고 Edit을 누릅니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/3-1_V3.png" />


><img src="https://developer.thebackend.io/static/img/unity/push_ios/3-2_V2.png" />

Push Notification을 확인해 보면 개발용 인증서와 배포용 인증서를 만들 수 있는 것을 확인할 수 있습니다.  
설명을 위한 테스트이기에 개발용으로 생성합니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/3-3_V2.png" />


1에서 생성한 **CertificateSigningRequest** 파일을 업로드하는 창이 나옵니다. Choose File 버튼을 클릭하여 추가해 줍니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/3-4_V2.png" />


인증서가 완성되었습니다. 다운로드합니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/3-5_V3.png" />


### 2-2 뒤끝 서버용 APNS 인증서 발급

><img src="https://developer.thebackend.io/static/img/unity/push_ios/4-1.png" />

  


><img src="https://developer.thebackend.io/static/img/unity/push_ios/4-2.png" />


완료 후, 해당 p12 파일을 뒤끝 콘솔 > 서버 설정 > 푸시 > iOS에서 p12 파일을 업로드하고, 설정한 암호를 입력합니다. 

><img src="https://developer.thebackend.io/static/img/unity/push_ios/4-3.png" />


### 2-3 프로비저닝 생성

Xcode에서 앱을 개발할 때 사용할 프로비저닝을 생성해 줍니다. Certificates, Identifiers & Profiles 페이지 좌측 패널 하단에 Provisioning Profiles > All을 선택하고 +버튼을 눌러 새로운 프로파일을 추가해 줍니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/5-1_V2.png" />


iOS App Development를 선택 후 계속 버튼을 눌러줍니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/5-2_V2.png" />


계속해서 미리 생성해두었던 App Id를 선택 후 Continue를 클릭합니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/5-3_V2.png" />


인증서를 선택해 줍니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/5-4_V2.png" />


테스트를 진행할 기기를 선택합니다. 푸시 알림의 경우 에뮬레이터로 확인할 수 없어 반드시 기기를 등록해 확인해야 합니다.  
다음 페이지에서 프로필 이름을 작성해 주면 프로비저닝이 완성됩니다.  
다운로드 버튼을 클릭하여 프로비저닝을 다운로드하고, 다운로드 된 파일을 더블클릭하면 Xcode에 자동으로 적용됩니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/5-5_V2.png" />

## 3. Xcode 프로젝트 설정하고, 빌드 하기

프로젝트 설정 창 상단에서 Build Setting을 클릭 후 우측 검색창에서 code signing을 검색합니다.  
Debug 우측 창을 클릭하여, 설정해두었던 Push service 인증서를 선택해 줍니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/6-1_V2.png" />


좌측 패널에서 Targets 밑에 있는 앱 이름을 클릭합니다. Capability 옵션에서 Push Notification 기능을 켜줍니다.  

><img src="https://developer.thebackend.io/static/img/unity/push_ios/6-2_V2.png" />
