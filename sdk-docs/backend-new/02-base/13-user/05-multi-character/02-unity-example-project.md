---
sidebar_label: 멀티 캐릭터 예제 게임
---

# 멀티 캐릭터 예제 게임

:::caution

- 멀티 캐릭터 기능에 여러 가지 개선이 필요한 상황임을 확인하고, 현재는 멀티 캐릭터 프로젝트 생성을 제한한 상태입니다. 이미 멀티 캐릭터를 사용 중인 프로젝트에서는 계속 멀티 캐릭터 기능을 이용하실 수 있습니다.
- 만일 멀티 캐릭터 기능을 사용하지 않고 일반 프로젝트로 전환을 원하시는 경우, help@backnd.com로 문의해 주세요.
- 보다 편리하고 안정적으로 사용하실 수 있도록 정비 중이오니 너른 양해 부탁드립니다.

:::

## 개요

해당 프로젝트는 Backend SDK 5.10.0에 추가된 멀티 캐릭터 로그인의 로그인 절차를 간단하게 나타낸 프로젝트입니다.

### 다운로드

:::danger 뒤끝 SDK 5.10.0, 5.10.1 이용 불가
뒤끝 SDK 5.10.0, SDK 5.10.1은 함수가 작동되지 않는 에러가 발생하여 이용이 불가능합니다. 해당 오류의 수정 버전인 SDK-5.10.2을 이용해주시기 바랍니다.  
:::

[뒤끝베이스 튜토리얼 다운로드](https://developer.thebackend.io/sdk/script/multiCharacter/5.10.2/BackendMultiCharacter.unitypackage)

### 인게임

**로그인화면**
<img src="https://developer.thebackend.io/static/img/unity/multiCharacter/example/loginScene.png"/>

해당 씬에서는 다음과 같은 뒤끝 함수를 호출합니다.

- 유저 계정 로그인(멀티 캐릭터용)
- 유저 계정 회원가입(멀티 캐릭터용)

**캐릭터 선택 화면**
<img src="https://developer.thebackend.io/static/img/unity/multiCharacter/example/characterSelectScene-1.png"/>
<img src="https://developer.thebackend.io/static/img/unity/multiCharacter/example/characterSelectScene-2.png"/>

해당 씬에서는 다음과 같은 뒤끝 함수를 호출합니다.

- 캐릭터 리스트 불러오기
- 캐릭터로 로그인하기
- 캐릭터 생성하기
- 캐릭터 삭제하기
- 계정 로그아웃하기
- 계정 탈퇴하기

**인게임 화면**
<img src="https://developer.thebackend.io/static/img/unity/multiCharacter/example/inGameScene.png"/>

해당 씬에서는 다음과 같은 뒤끝 함수를 호출합니다.

- 게임 정보 삽입

## 설정

해당 프로젝트를 정상적으로 이용하기 위해서는 2가지 준비가 필요합니다.

1. 뒤끝 콘솔에서 USER_PROFILE 테이블 만들기
2. 유니티 import 후, 씬 3개 등록하기

### USER_PROFILE 테이블 생성하기

뒤끝 콘솔에 들어가 게임 정보 관리에서 테이블 생성을 클릭합니다.  
테이블 이름을 **USER_PROFILE**로 지정합니다.(스키마는 미정의로 설정)
<img src="https://developer.thebackend.io/static/img/unity/multiCharacter/example/table-setting.png"/>

### UNITY 설정

1. 유니티 패키지파일 import 후, `Assets > Scene`에 존재하는 LoginScene, CharacterSelectScene, InGameScene 3개의 씬을 등록합니다.  
   씬 등록은 유니티 상단 바의 `File > Build Settings > Scenes in Build` 칸에서 추가 가능합니다.  
   최초 씬은 LoginScene으로 설정합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/multiCharacter/example/setting1.png"/>

2. Game 뷰에서 해상도를 Full HD 혹은 1920 x 1080으로 설정합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/multiCharacter/example/setting2.png"/>

3. 유니티 상단 The Backend > Edit Settings를 클릭하여 뒤끝 콘솔에 존재하는 Client App Id와 Signature Key를 입력합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/multiCharacter/example/clientAppId.png"/>
