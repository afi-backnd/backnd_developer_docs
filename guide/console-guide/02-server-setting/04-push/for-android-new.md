import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# Android 푸시 등록

안드로이드(구글 플레이스토어, 원스토어) 에서 뒤끝 콘솔에서 보내는 푸시를 받는 기능을 제공합니다. 


- 다국어 지원
  - 유저의 국가 코드가 존재하고, 콘솔에 해당 국가 푸시가 등록되어 있는 경우 → 해당 국가의 푸시를 발송합니다.  
  - 유저의 국가 코드가 존재하지 않거나, 콘솔에 해당 국가 푸시가 등록되어 있지 않은 경우 → 기본 설정의 푸시를 발송합니다.  

## 푸시를 받기 위한 [프로젝트 등록](https://firebase.google.com/)

### 패키지 네임 설정
먼저 파이어베이스 푸시와 뒤끝 콘솔 모두 해당 앱의 packagename이 필요합니다.
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-1.png)


### Firebase 프로젝트 생성
뒤끝은 Firebase의 FCM을 이용해 푸시 서비스를 제공합니다. 뒤끝 푸시 서비스를 이용하기 위해서 우선 Firebase에 프로젝트를 등록해야 합니다.  

![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-2.png)
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-3.png)
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-4.png)
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-5.png)


### Firebase 프로젝트 내 안드로이드 설정
프로젝트가 생성된 이후에는 해당 프로젝트 내에서 안드로이드 환경을 설정합니다.

![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-6.png)
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-7.png)
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-8.png)

### google-service 파일 유니티에 등록
안드로이드 설정이 완료된 이후에는 안드로이드 설정에 진입합니다.

![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-9.png)


**google-service.json**을 다운로드해 Unity 프로젝트의 Assets 내부에 추가합니다. 이때, 파일의 이름은 google-service로 고정되어야 합니다.

![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-10.png)
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-11.png)


### 클라우드 메시징 설정
생성된 프로젝트의 설정 페이지에서 클라우드 메시지 탭으로 들어오면 Firebase Cloud Messaging API(V1) 칸이 존재합니다.  
해당 칸에 존재하는 **서비스 계정 관리** 버튼을 클릭합니다.

![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-12.png)

이후로는 Google Cloud Platform의 프로젝트로 이동하게 됩니다.  
자동으로 생성된 서비스 계정을 클릭합니다.

![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-13.png)

**키** 탭으로 이동합니다.  
**키 추가** 버튼을 클릭합니다

![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-14.png)
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-15.png)

Json 형식으로 선택합니다.
![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-16.png)

선택이 완료되면 자동으로 json 파일이 다운로드 됩니다.  
해당 파일을 뒤끝 콘솔에 업로드합니다.  

<ConsoleLinkButton text="푸시 알림 설정 바로가기" menu="settingPush" feature="푸시 알림 설정" title="Android 푸시 등록" />

![](/img/docs/guide/console-guide/server-setting/push/for-android-new/android-push-new-17.png)

## 유니티

Firebase에서 제공하는 [UnitySDK](https://firebase.google.com/docs/unity/setup?authuser=0)를 다운로드합니다. 다운로드한 SDK에서 FirebaseMessaging.unitypackage를 unity 프로젝트에 import 합니다.  

<img src="https://developer.thebackend.io/static/img/unity/push_and/8.png" />
