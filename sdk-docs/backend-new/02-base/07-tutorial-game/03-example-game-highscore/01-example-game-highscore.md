---
sidebar_label: "BackEndHero : 하이스코어"
description: "BackEndHero : 하이스코어"
sidebar_position: 6.5
---

# BackEndHero

BackendHero는 게임을 개발할 때 도움이 될 수 있도록 뒤끝에서 제공하는 예제 게임입니다.  
[므쮸 컴퍼니](http://mmzzuu.com/)에서 게임을 제작하였고, 뒤끝 SDK를 사용하여 서버 기능을 구현하였습니다. 관련 소스코드는 모두 오픈소스로 공개됩니다.  
BackEndHero는 **Unity 2020.1.3f1**과 **Backend-5.9.0**을 기준으로 개발되었습니다.  

## 라이센스

BackEndHero는 [BSD-2-Clause](https://github.com/thebackend-io/BackEndHero/blob/master/LICENSE) 라이센스를 따릅니다.  

## 마켓 링크

- [Apple App Store](https://apps.apple.com/us/app/backend-hero/id1461432877?l=ko&ls=1)
- [Google Play Store](https://play.google.com/store/apps/details?id=com.afi.BackEndHero)

## 유니티 패키지 파일

- [BackEndHero.unitypackage](https://developer.thebackend.io/sdk/script/tutorial/5.9.0/BackendHero.unitypackage)

## 스크린샷

> ![image](https://developer.thebackend.io/static/img/sampleGameGuide/gameInfo.png)

## 포함된 기능

BackEndHero는 아래의 뒤끝베이스와 뒤끝챗 기능들을 포함하고 있습니다.  
사용된 뒤끝 기능은 뒤끝의 모든 기능이 아닌, 뒤끝 기능의 일부이며 추후 BackEndHero 예제 게임에 추가될 수 있습니다.  

### 뒤끝 베이스 기능

| 기능           | 설명                                                     |
| -------------- | -------------------------------------------------------- |
| 커스텀 계정    | 커스텀 회원가입 및 로그인                                |
| GPGS 로그인    | GPGS 로그인 토큰을 이용한 페더레이션 회원가입 및 로그인  |
| Apple 로그인   | Apple 로그인 토큰을 이용한 페더레이션 회원가입 및 로그인 |
| 토큰 로그인    | 뒤끝 액세스 토큰을 이용한 토큰 로그인                    |
| 닉네임         | 닉네임 생성 / 수정                                       |
| 게임 정보 관리 | 유저 점수 / 구매 내역 업데이트 및 조회                   |
| 공지사항       | 공지사항 조회하기                                        |
| 1대1 문의      | 1대1 문의 하기, 문의 내역 조회                           |
| 일반 랭킹      | 1 ~ 10 위까지의 랭킹 조회, 내 랭킹 및 점수 조회          |
| 푸시           | 푸시 활성화 / 비활성화                                   |
| 영수증 검증    | 광고 제거 아이템 구매 영수증 검증                        |
| 로그           | 최고 점수 달성 로그, 광고 제거 아이템 구매 로그 기록     |

### 뒤끝챗 기능

| 기능                       | 설명                                        |
| -------------------------- | ------------------------------------------- |
| 채팅 서버 활성화 상태 확인 | 뒤끝챗 활성화 상태 확인                     |
| 그룹 채널 리스트           | 채널 리스트 조회                            |
| 최근 메시지 조회           | 최근 메시지 조회                            |
| 채팅 서버 접속 / 접속 종료 | 채팅 서버 접속 및 접속 종료                 |
| 메시지 송수신              | 메시지 송수신                               |
| 귓속말 송수신              | 귓속말 송수신                               |
| 유저 차단                  | 특정 유저 차단 / 차단 해제                  |
| 유저 신고                  | 특정 유저 신고                              |
| 필터링                     | 비속어 필터링                               |
| 도배 방지                  | 도배 방지 메시지 설정                       |
| 자동 접속 종료             | 일정 시간 메시지를 안 보냈을 경우 접속 종료 |

## 예제 게임 실행 방법

### 1. 상단의 유니티 패키지 파일을 다운로드합니다.  

### 2. 유니티에서 해당 프로젝트를 불러옵니다.  

- 임포트 시, Project Settings의 Physic 항목도 같이 변경됩니다.  

### 3. **Assets > Scenes**의 Game을 엽니다.  

### 4. 뒤끝 콘솔에서 새 프로젝트를 생성합니다.  

해당 프로젝트에서 Client App ID와 Signature Key를 발급받습니다. [해당 문서](/sdk-docs/backend/base/start-up)를 참고하시면 됩니다.  
이후 발급받은 Client App ID와 Signature Key를 **The Backend > Edit Setting**에 입력합니다.  

- 뒤끝 SDK의 경우 이미 소스코드에 포함되어 있습니다. 별도로 설치하지 않으셔도 됩니다.  

### 5. 데이터 저장을 위해 테이블을 생성합니다.  

콘솔의 좌측 메뉴에 **게임 정보 관리 > 테이블 관리**에서 **테이블 만들기**버튼을 클릭합니다.  
**DB버전 선택**을 **신버전(SDK 5.4.0 이상)**으로 선택하고 score테이블과 purchase테이블을 다음과 같이 생성합니다.  

#1
분류 : Private
테이블 명 : score
테이블 설명 : 원하는 내용
스키마 정의 : 미사용
상태 : 활성

#2
분류 : Private
테이블 명 : purchase
테이블 설명 : 원하는 내용
스키마 정의 : 미사용
상태 : 활성

이후 랭킹의 컬럼 생성을 위해 게임을 플레이하여 스코어를 업데이트합니다.  
(실제 게임플레이를 통해 게임에서 패배할 경우, 내부로직에 의해 score 테이블에 데이터가 삽입/수정됩니다.)

<img src="https://developer.thebackend.io/static/img/sampleGameGuide/gamedata-1.png" />

### 6. 랭킹 확인을 위해 랭킹을 생성해 줍니다.  

콘솔의 좌측 메뉴에 **랭킹 관리 > 랭킹**에서 새 랭킹을 생성해 줍니다.  

유형 : 유저 랭킹
랭킹 명 : 원하는 이름
초기화 기간 : 누적랭킹
랭킹 항목 : 테이블 - score, 컬럼 - score
추가항목 : owner_inDate(혹은 score 외에 원하는 컬럼)
랭킹 보상 : 없음(선택불가)

랭킹 생성 시 score 컬럼을 선택할 수 없는 경우 게임을 플레이하여 게임 정보를 업데이트한 뒤, 랭킹을 생성합니다.(게임에서 패배하면 자동으로 게임 정보가 업데이트됩니다.)

<img src="https://developer.thebackend.io/static/img/sampleGameGuide/rank-1.png" />

생성된 랭킹을 클릭하여 상단의 uuid를 복사합니다.  

<img src="https://developer.thebackend.io/static/img/sampleGameGuide/rank-2.png" />

해당 uuid를 유니티의 하이 라커에서 BackEnd Server Manager 객체를 클릭하고, 우측 인스펙터 패널에서 Back End Server Manager 스크립트에 Rank Uuid에 입력합니다.  

<img src="https://developer.thebackend.io/static/img/sampleGameGuide/rank-3.png" />

### 7. 뒤끝챗을 활성화하고 채널 그룹을 생성합니다.  

1. 콘솔의 좌측 메뉴에 **뒤끝챗**을 선택해 뒤끝챗을 활성화합니다.  

2. 이후 채널 그룹 관리에서 새 채널 그룹을 만들고 분류를 **Public**, **채널 자동 생성**을 사용으로 그룹을 생성합니다.  

> ![image](https://developer.thebackend.io/static/img/sampleGameGuide/5.png)

마지막으로 생성된 그룹의 이름을 랭킹과 같이 유니티의 하이 라커에서 BackEnd Server Manager 객체를 클릭하고, 우측 인스펙터 패널에서 Back End Server Manager 스크립트에 Group Name에 입력합니다.  

> ![image](https://developer.thebackend.io/static/img/sampleGameGuide/6.png)

### 8. 실행 시 Debug.Log가 작동하지 않을 수 있습니다.  

Debug.Log가 작동하지 않을 시 **File > Build Settings**에서 **Player Settings**를 클릭하고, **Other Settings > Scripting Define Symbols**에 **ENABLE_LOG**를 입력하시면 됩니다.  

> ![image](https://developer.thebackend.io/static/img/sampleGameGuide/7.png)

### 9. InAppPurchase 관련 기능에서 오류가 발생할 수 있습니다.  

Sevices 탭에서 새 프로젝트를 생성하고, **In-App Purchasing** 항목을 **ON** 시켜준 후 프로젝트를 재시작하면 오류가 제거됩니다.  

- 유니티 내에 오류 로그가 없을 경우에만 정상적으로 인앱 기능이 import 됩니다.  

> ![image](https://developer.thebackend.io/static/img/sampleGameGuide/8.png)

## 작동하지 않는 기능

예제 게임 프로젝트를 실행하면 GPGS 회원가입 기능, 푸시 기능, 구매 관련 기능, 광고 기능이 정상적으로 작동하지 않습니다.  
해당 기능은 관련 SDK를 별도로 다운로드해야 하며, 구글 및 애플 인증과 유니티 광고 ID가 노출되는 문제가 있어 소스코드만 공개하고 관련 ID는 모두 제거하였습니다.  
상단의 표시한 구글 Play Store 및 애플 App Store에서 앱을 다운로드해 해당 기능을 테스트해볼 수 있습니다.  
또한 해당 기능은 아래 문서를 참고하셔서 구글 및 애플의 인증을 받으면 개발자분들이 직접 테스트를 해볼 수 있습니다.  

- [GPGS 페더레이션 회원가입](/sdk-docs/backend/base/user/federation/example-using-gpgs)
- [안드로이드 푸시 설정](/guide/console-guide/server-setting/push/for-android)
- [iOS 푸시 설정](/guide/console-guide/server-setting/push/for-ios)
- [구글 영수증 검증](/sdk-docs/backend/base/receipt/android/setting-google-console)
- [애플 영수증 검증](/sdk-docs/backend/base/receipt/ios/setting-google-console)
