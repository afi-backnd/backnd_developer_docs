---
description: "Android 푸시 등록(구버전)"
---

# Android 푸시 등록(구버전)

안드로이드(구글 플레이스토어, 원스토어) 에서 뒤끝 콘솔에서 보내는 푸시를 받는 기능을 제공합니다. 


- 다국어 지원
  - 유저의 국가 코드가 존재하고, 콘솔에 해당 국가 푸시가 등록되어 있는 경우 → 해당 국가의 푸시를 발송합니다.  
  - 유저의 국가 코드가 존재하지 않거나, 콘솔에 해당 국가 푸시가 등록되어 있지 않은 경우 → 기본 설정의 푸시를 발송합니다.  

## 푸시를 받기 위한 [프로젝트 등록](https://firebase.google.com/)

뒤끝은 Firebase의 FCM을 이용해 푸시 서비스를 제공합니다. 뒤끝 푸시 서비스를 이용하기 위해서 우선 Firebase에 프로젝트를 등록해야 합니다.  

<img src="https://developer.thebackend.io/static/img/unity/push_and/android_push_1.png" />
<img src="https://developer.thebackend.io/static/img/unity/push_and/android_push_2.png" />

  

<img src="https://developer.thebackend.io/static/img/unity/push_and/android_push_3.png" />

  


<img src="https://developer.thebackend.io/static/img/unity/push_and/android_push_4.png" />

**google-service.json**을 다운로드해 Unity 프로젝트의 Assets 내부에 추가합니다.  

<img src="https://developer.thebackend.io/static/img/unity/push_and/android_push_5.png" />

생성된 프로젝트의 설정 페이지에서 클라우드 메시지 탭으로 들어오면 Cloud Messaging API가 존재합니다.  
처음 생성하였을 경우, 비활성화가 되어 있을 수 있습니다.  
이와 같은 경우에는 오른쪽 '더보기' 버튼을 클릭합니다.  

<img src="https://developer.thebackend.io/static/img/unity/push_and/android_push_7.png" />

  


Cloud Messaging에 들어가 '사용' 버튼을 클릭합니다.  

<img src="https://developer.thebackend.io/static/img/unity/push_and/android_push_8.png" />

  


이후 Cloud Messaging API(기존)이 사용 설정됨으로 표시되면서 서버 키를 얻을 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/unity/push_and/android_push_9.png" />


해당 서버키를 뒤끝 콘솔 - 푸시 - Android의 API Key에 붙여넣기합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정푸시-안드로이드-서버-키.png" />

Firebase에서 제공하는 [UnitySDK](https://firebase.google.com/docs/unity/setup?authuser=0)를 다운로드합니다. 다운로드한 SDK에서 FirebaseMessaging.unitypackage를 unity 프로젝트에 import 합니다.  

<img src="https://developer.thebackend.io/static/img/unity/push_and/8.png" />
