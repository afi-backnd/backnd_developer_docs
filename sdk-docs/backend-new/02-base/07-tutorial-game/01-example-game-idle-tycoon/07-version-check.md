---
sidebar_label: 버전 관리
description: "버전 관리"
sidebar_position: 7
---

# 프로젝트 버전 관리

게임을 Android 환경에서 빌드해 실행할 경우 디바이스에 설치된 APK의 버전과 뒤끝 콘솔에 등록된 버전을 확인합니다.  
뒤끝 콘솔에 버전이 등록 되어 있지 않으면 안드로이드 환경에서는 설치된 APK의 버전을 확인 할 수 없기 때문에 게임 플레이가 불가능 합니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/versionCheck.png)

[버전 관리](/guide/console-guide/backnd-base/versions/) 페이지를 참고하여 뒤끝 콘솔에 버전을 등록을 해야 게임 플레이가 가능합니다.  
  
프로젝트 내 `BackndServerState.cs` 파일의 `CheckVersionStatus()` 함수에서 현재 실행 중인 클라이언트 버전과 콘솔에 등록된 버전을 비교하여, 플레이 가능 여부를 판단합니다.  
기능의 자세한 내용은 [프로젝트 버전 조회](/sdk-docs/backend/base/sdk-utils/get-project-version/)에서 확인 할 수 있습니다.  
