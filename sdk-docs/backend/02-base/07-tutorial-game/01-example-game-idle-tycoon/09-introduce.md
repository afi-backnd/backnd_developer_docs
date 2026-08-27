---
sidebar_label: "구현 기능 설명"
sidebar_position: "9"
description: "구현 기능 설명"
---

# 구현 기능 설명

예제 게임 ‘Food Truck Venture’에 적용된 뒤끝 베이스의 주요 기능들을 설명합니다.

:::info
이 문서는 Food Truck Venture 예제에 포함된 모든 뒤끝 기능의 구조와 사용 위치를 안내합니다.  
각 항목에 연결된 상세 문서를 참고하시면, 실제 구현 및 커스터마이징에 필요한 모든 정보를 확인하실 수 있습니다.

추가로 궁금한 점이 있다면 뒤끝 커뮤니티 또는 공식 SDK 문서에서 검색해 보세요!
:::


## 유저 관리

### 게스트 및 페더레이션 로그인 / 회원가입

게임을 플레이 하기 위해 플레이어는 게스트 계정을 사용하거나 페더레이션 인증(Google / Apple)을 통해 게임에 접속할 수 있습니다.  
플레이어가 회원가입하면, 이후 게임을 다시 실행할 때 자동으로 로그인됩니다.  

게스트 계정에 관한 정보는 [게스트 계정](/sdk-docs/backend/base/user/guest/signup-and-login)에서 확인 가능합니다.  
페더레이션 인증의 경우, Android환경에서의 로그인 관련 정보는 [구글 로그인 설정](/sdk-docs/backend/toolkit/google-login/install-sdk)에서 확인 가능하며  
iOS 환경인 경우에는 [Apple 로그인 인증 예제](/sdk-docs/backend/base/user/federation/example-using-apple)를 참고하시기 바랍니다.  

### 계정 전환
사용 플랫폼에 따라 게스트 계정에서 Google 또는 Apple 계정으로 변경을 원하는 경우에는 게임 내에서 로그인 변경 옵션을 제공합니다.  
페더레이션 계정이 이미 가입되어있는 계정인지 체크한 뒤 가입되어 있지 않은 계정일 경우 계정 변환을 시작합니다.  
게스트 계정에서 페더레이션 계정으로 전환에 대한 부분은 [페더레이션 계정 전환](/sdk-docs/backend/base/user/federation/migrate-from-custom)을 참고하시기 바랍니다.  

프로젝트에서 게스트 및 페더레이션 로그인 / 회원가입이 구현된 부분은 아래에 명시된 파일과 함수를 확인해 주세요.  
```
BackndLogin.cs    //파일명
├── GuestLogin    //함수명
├── GoogleLogin
├── AppleLogin  
├── CustomSignUp  
├── CheckUserInBackend  
├── ChangeCustomToFederationLogin  
└── ChangeCustomToFederationLoginApple  
```

### 토큰 로그인

이전에 게스트 또는 구글 로그인을 시도한 적이 있는 상태에서 게임을 실행 했을 때, 게임 타이틀 화면에서 토큰 로그인을 시도합니다.  
토큰 로그인이 성공할 경우, 로딩씬으로 이동하며 게임이 시작됩니다.  
토큰 로그인에 대한 부분은 [유저 인증 토큰](/sdk-docs/backend/base/user/token/accesstoken)을 참고하시기 바랍니다.  

프로젝트에 토큰 로그인이 구현되어 있는 부분은 아래와 같습니다.  
```
BackndLogin.cs                 //파일명
└── IsLoginWithTokenSuccess    //함수명
```

### 닉네임

플레이어의 닉네임을 설정합니다.  
닉네임 설정 시 중복 되지 않는 닉네임으로만 설정 가능합니다.  
닉네임 수정에 대한 부분 [닉네임 수정](/sdk-docs/backend/base/user/nickname/update)에서 확인 가능합니다.

닉네임 수정이 구현되어 있는 부분은 아래와 같습니다.  
```
BackndUserInfo.cs    //파일명
└── SetUserName      //함수명
```

### 유저 정보 가져오기 
GetUserInfo 함수를 사용하여 뒤끝 콘솔에서 유저 정보를 얻어옵니다.  
더 자세한 설명은 [유저 정보 가져오기](/sdk-docs/backend/base/user/get-my-information/get-from-server-v2)에서 확인 가능합니다.

유저 정보 가져오기 부분이 구현되어 있는 부분은 아래와 같습니다.  
```
BackndUserInfo.cs    //파일명
└── LoadData         //함수명
```
----------------------------------------------------------------------------------------------------------

## CDN 차트, 확률

### 차트 관리 
게임 내 공용 데이터를 저장하는 용도로 사용되며, 해당 데이터는 뒤끝 콘솔에서 직접 수정되지 않는 한 변경할 수 없습니다.  
불러온 차트 데이터는 게임 내 스크립터블 오브젝트에 저장되어 사용됩니다.  

게임 내 차트에서 다루는 데이터의는 아래와 같습니다.  

주방 기기의 업그레이드 : 비용, 수익, 각 단계의 최대 레벨 도달 시 제공되는 보너스  
스테이지 업그레이드 : 현재 스테이지에서 영구적인 버프 제공  
게임 내 모든 아이템 : 아이템 데이터를 저장하여 게임 내 교차 검증 및 우편 시스템을 통해 아이템 지급 시 활용  

구현된 차트의 자세한 사용 설명은 [차트 테이블 조회](/sdk-docs/backend/base/chart/chart-table)와 [차트 내용 조회](/sdk-docs/backend/base/chart/chart-content)에서 확인 해주세요.  

차트를 불러오는 부분과 스크립터블 오브젝트에 저장하는 부분이 구현된 부분은 아래와 같습니다.  
```
차트 불러오기
BackndChart.cs       //파일명
└── GetChartDatas    //함수명

스크립터블 오브젝트에 저장
SO_ChartData.cs 
```

### 확률 관리

플레이어가 특정 재화를 사용하여 확률형 아이템을 구매할 경우 확률에 따라서 도출된 등급과 종류의 아이템을 지급하게 됩니다.  

구현된 차트의 자세한 설명은 [확률 테이블 조회](/sdk-docs/backend/base/probability/probability-table)와 [뽑기 확률 내용 조회](/sdk-docs/backend/base/probability/probability-content)에서 확인 해주세요.  

```
확률 불러오기
BackndProbability.cs             //파일명
└── GetProbabilityContentData    //함수명

스크립터블 오브젝트에 저장
SO_ProbabilityData.cs 
```
----------------------------------------------------------------------------------------------------------

## 게임 정보 관리

### 게임 정보 삽입, 조회, 수정, 삭제

게임 내에서 플레이어의 진행 상황을 저장하기 위해 서버에 동적으로 기록되는 데이터입니다.  
플레이어의 보석 보유량, 누적 로그인 일수, 업그레이드, 특정 이벤트 달성 여부 등의 정보가 포함됩니다.  
[게임 정보 관리](/sdk-docs/backend/base/game-information/insert)부분을 참고하여 플레이어의 상태를 안정적으로 저장하고, 언제든지 불러올 수 있도록 관리합니다.  

```
UserDailyLoginDataManager.cs  //파일명

UserStageDataManager.cs

UserDataManager.cs

UserWardrobeDataManager.cs
```

### 자동 저장

AutoSaveTimer.cs 내부에서 타이머가 동작하여 지정된 시간 마다 자동 저장을 시도합니다.
액션 함수인 OnSaveTime에 Invoke를 호출하여 OnSaveTime에 등록된 SaveData함수들을 호출하여 저장을 시도합니다.  
```
UserDailyLoginDataManager.cs  //파일명
└──SaveData                   //함수명

UserStageDataManager.cs
└──SaveData

UserDataManager.cs
└──SaveData

UserWardrobeDataManager.cs
└──SaveData

AutoSaveTimer.cs
└── Update        //OnSaveTime.Invoke
```
----------------------------------------------------------------------------------------------------------
## 유저 리더보드

리더보드는 모든 플레이어의 스코어를 비교하여 경쟁을 유도하는 기능 입니다.  
Foodtruck Venture는 유저 리더보드를 사용하여 골드 획득량을 기준으로 플레이어 간 경쟁이 이루어집니다.

프로젝트 실행을 위한 설명은 [데이터 테이블 및 리더보드 설정](/sdk-docs/backend/base/tutorial-game/example-game-idle-tycoon/table-and-leaderboard)에서 확인 가능하며,  
자세한 사용 방법에 관한 내용은 [유저 리더보드](/sdk-docs/backend/base/leaderboard/difference-to-rank)를 확인해 주세요.  

```
BackndLeaderboard.cs        //파일명
├── LoadLeaderBoard         //함수명
├── UpdateLeaderboard       
├── UpdateLeaderboardAsync
└── GetMyDataInLeaderboard
```

----------------------------------------------------------------------------------------------------------
## 우편 시스템

서버에서 플레이어에게 정보를 전달하는 용도로 사용됩니다.  
보상 지급 또는 중요한 공지 사항을 유저에게 전달 할 때 우편을 활용합니다.  
유저가 직접 우편을 보내는 기능은 구현되어 있지 않습니다.  

프로젝트 실행을 위한 설명은 [우편, 공지 설정 방법](/sdk-docs/backend/base/tutorial-game/example-game-idle-tycoon/post-and-notice)통해 확인 할 수 있습니다.

기능에 대한 더 자세한 내용은 [우편 기능](/sdk-docs/backend/base/post/difference-to-old) 확인해 주세요.  

```
BackndMail.cs     //파일명
├── LoadMailData  //함수명
├── ClaimMail
└── DeleteMail

AbstractMailReceiveReward.cs
```

----------------------------------------------------------------------------------------------------------
## 인앱 결제, 영수증 검증

인앱 결제를 통해서 유저가 아이템을 구매했는지 확인하고 각 구매 내역을 영수증 검증을 통해 확인합니다.  
구매 복원 기능을 통해서 게임 내 구매가 정상적으로 이루어졌는지 확인합니다.  

게임 내 인앱 결제 상품은 아래와 같습니다.
 
소모성 아이템 : 보석(Gems)  
비소모성 아이템 : 번들(Bundle) 및 코스튬(Costumes)

구매 복원 기능은 한 번만 구매 가능한 아이템을 여러 번 구매하는 것을 방지 할 수 있습니다. (기기 변경 또는 결제 오류 발생시)

자세한 내용은 예제 게임에 [인앱 결제, 영수증 검증 설정](/sdk-docs/backend/base/tutorial-game/example-game-idle-tycoon/IAP-and-receipt)페이지를 참고하세요.

----------------------------------------------------------------------------------------------------------

## 프로젝트 상태 확인
현재 게임이 플레이 가능한 상태인지 점검 중인지 확인 할 수 있으며, 서버 상태에 따라서 게임 접속을 허용할지 결정합니다.  
자세한 내용은 [프로젝트 상태 조회](/sdk-docs/backend/base/sdk-utils/get-project-status/)에서 확인하실 수 있습니다.  

```
BackndServerState.cs  //파일명
└── CheckServerStatus //함수명
```

## 버전 관리

뒤끝 콘솔을 통해 게임 버전을 제어하며, 특정 버전이 아니면 게임을 실행할 수 없도록 강제 업데이트를 유도 할 수 있습니다.  
자세한 내용은 [버전 관리](/sdk-docs/backend/base/tutorial-game/example-game-idle-tycoon/version-check) 페이지를 참고해 주세요.

```
BackndServerState.cs    //파일명
└── CheckVersionStatus  //함수명
```
----------------------------------------------------------------------------------------------------------
## 공지사항과 임시공지

유저에게 게임 내 공지 사항을 전달 때 사용합니다.  
유지보수 일정 안내 혹은 게임 업데이트, 기타 이슈 등의 여러가지 전달 사항들을을 표시 할 수 있습니다.  
임시공지는 일반 공지사항과 별개의 공지사항이고 1건만 가능합니다.  
로그인하지 않은 유저에게도 공지를 전달 할 수 있습니다.  

프로젝트 실행을 위한 작업은 [우편, 공지 설정 방법](/sdk-docs/backend/base/tutorial-game/example-game-idle-tycoon/post-and-notice)통해 설정 할 수 있습니다.  

구현을 위한 자세한 내용은 [공지사항 리스트 조회](/sdk-docs/backend/base/operation/notice/get-list)와 [임시 공지 조회](/sdk-docs/backend/base/operation/notice/get-temporary-one)를 참고해 주세요.

```
BackndAnnouncement.cs   //파일명
├── LoadAnnouncement     //함수명
└── GetTemporaryNotice

AbstractMailReceiveReward.cs
```
