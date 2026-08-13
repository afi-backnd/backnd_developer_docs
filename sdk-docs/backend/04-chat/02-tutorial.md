---
sidebar_label: "튜토리얼"
draft: "true"
unlisted: "true"
description: "튜토리얼"
---

# 튜토리얼

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

뒤끝챗을 이용한 간단한 채팅 클라이언트 튜토리얼입니다.    

## 유니티 패키지 파일

[뒤끝챗 튜토리얼 다운로드](https://developer.thebackend.io/sdk/script/tutorial/5.9.0/BackendChat.unitypackage)

## 개발 환경

| 유니티 버전 | 뒤끝 SDK 버전 | 닷넷 버전 |
| ----------- | ------------- | --------- |
| 2020.1.3f1  | 5.9.0         | 닷넷 4    |

## 포함된 기능

| 포함된 기능                | 설명                                    |
| :------------------------- | :-------------------------------------- |
| 커스텀 회원가입 / 로그인   | 커스텀 계정 회원가입 / 로그인           |
| 닉네임                     | 닉네임 설정                             |
| 채팅 상태                  | 채팅 상태 조회                          |
| 서버 연결 확인             | 채팅 서버 연결 확인                     |
| 일반/길드 채널 입장 / 퇴장 | 일반 / 길드 채팅 채널 입장 및 퇴장      |
| 최근 채팅                  | 최근 채팅 불러오기                      |
| 채팅                       | 채팅 송수신                             |
| 귓속말                     | 귓속말 송수신                           |
| 운영자 채팅                | 운영자 전체 채팅 송수신                 |
| 차단                       | 특정 유저 차단 / 차단 해제              |
| 신고                       | 특정 유저 신고                          |
| 필터링                     | 비속어 필터링 활성화 / 비활성화         |
| 자동 접속 종료             | 일정 시간 채팅 미사용 시 자동 접속 종료 |
| 도배 방지                  | 채팅 도배 방지                          |

### 1. 뒤끝챗 활성화

뒤끝 콘솔 - 뒤끝챗 메뉴에서 하단의 **뒤끝챗 활성화** 버튼을 눌러 뒤끝챗을 활성화합니다.  

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/1.png)

### 2. 채널 그룹 생성(일반, 길드)

뒤끝챗 - 채널 그룹 관리 메뉴에서 **새 그룹 만들기** 버튼을 눌러 일반 채널과 길드 채널을 생성합니다.  
자동 채널 생성이 미사용일 경우, 채널 관리에서 채널을 직접 생성해야 합니다.  

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/2.png)

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/3.png)

### 3. 유니티 뒤끝 인스펙터 설정

뒤끝챗 예제를 임포트 하고 유니티 상단의 The Backend > Edit Settings메뉴를 통해서 프로젝트를 설정합니다.  

### 4. 유니티 씬 설정

유니티 내 Project에 `Assets > TheBackend > Example > chat` 폴더 내에 ChatTest-login 씬과 ChatTest-channellist 씬을 등록합니다.  
유니티 상단 메뉴 File - BuildSettings에 Add Open Scenes 버튼을 눌러 현재 씬을 등록할 수 있습니다.  

- ChatTest-login 씬이 0번, ChatTest-channellist 씬이 1번이 되어야 하며, 시작 씬은 ChatTest-login 씬입니다.  

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/4.png)

### 5. 로그인

프로젝트를 플레이하여 로그인을 하거나 회원가입을 합니다.  

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/5.png)

### 6. 채널 그룹 검색

검색하고자 하는 채널 타입(일반, 길드)을 선택하고 그룹 이름을 입력한 뒤 검색 버튼을 누릅니다.  

- 뒤끝챗은 일반 채널과 길드 채널 동시에 채팅을 주고받을 수 있습니다.  

검색 후, 채널 오브젝트들이 생성되면 해당 오브젝트에 **입장**을 눌러 입장하실 수 있습니다.  

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/6.png)

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/7.png)

### 7. 메시지 보내기

입장 후, 하단의 입력창을 입력한 후 메시지 보내기 버튼을 눌러 메시지를 보낼 수 있습니다.  

채팅 시,
입력창에 명령어를 입력할 수 있습니다.  
명령어는 다음과 같습니다.(따옴표 제거)

| 기능             | 명령어                                            | 예시                              |
| ---------------- | ------------------------------------------------- | --------------------------------- |
| 귓속말           | /w "닉네임" "해당 닉네임의 유저에게 하고 싶은 말" | /w timmy 정원에서 봐~             |
| 운영자 전체 채팅 | /all "입력할 공지사항"                            | /all 오늘 4시에 서버를 점검합니다 |
| 차단             | /b "닉네임"                                       | /b timmy                          |
| 차단해제         | /ub "닉네임"                                      | /ub timmy                         |
| 신고             | /신고 "닉네임"                                    | /신고 timmy                       |

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/8.png)

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/9.png)

### 8. 일반/길드 채널 전환

상단에 토글 박스를 클릭하여 일반 채널 또는 길드 채널로 전환하실 수 있습니다.  
일반 채널에 입장 후, 길드 채널에 입장하지 않았을 경우에는 길드 토글을 클릭하면 그룹 검색 화면으로 갑니다.  

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/10.png)

### 9 채널 나가기

대화창 내에 나가기 버튼을 눌러 채널을 나갈 수 있습니다.  

> ![image](https://developer.thebackend.io/static/img/unity/chat/tutorial/11.png)
