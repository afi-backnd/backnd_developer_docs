---
sidebar_label: 튜토리얼
description: "튜토리얼"
sidebar_position: 6.6
---

# 튜토리얼 프로젝트

뒤끝 기능 간편 테스트 및 게임을 개발할 때 도움이 될 수 있도록 뒤끝에서 제공하는 뒤끝베이스의 기능들을 포함하고 있는 프로젝트입니다.  
해당 프로젝트는 **Unity 2022.3.38f1**과 **Backend-5.18.2**을 기준으로 개발되었습니다.  

## 유니티 패키지 파일

- <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.3/BackendTutorial.unitypackage" id="download-sdk-base-tutorial">뒤끝베이스 튜토리얼 다운로드</a>

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter4-1.png" />

## 뒤끝 베이스 기능

해당 프로젝트는 뒤끝 외에 서비스에서 설정해야 하는 일부 기능을 제외하고는 모든 베이스 기능을 포함하고 있습니다.  
튜토리얼에 포함되지 않는 베이스 기능은 아래와 같으며, 해당 기능들은 뒤끝 예제 게임에서 확인하실 수 있습니다.  

## 튜토리얼에 제공되지 않는 기능

| 기능         | 설명                                                     |
| ------------ | -------------------------------------------------------- |
| GPGS 로그인  | GPGS 로그인 토큰을 이용한 페더레이션 회원가입 및 로그인  |
| Apple 로그인 | Apple 로그인 토큰을 이용한 페더레이션 회원가입 및 로그인 |
| 1대1 문의    | 1대1 문의 하기, 문의 내역 조회                           |
| 푸시         | 푸시 활성화 / 비활성화                                   |
| 영수증 검증  | 광고 제거 아이템 구매 영수증 검증                        |

## 사용 방법

### 1. 패키지 파일 임포트 및 씬 열기

BackendTutorial.unitypackage를 임포트 한 후, `Assets > Scenes` 에 있는 ExampleScene을 더블 클릭하여 엽니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter0.png" />

### 2. Client App ID 입력

뒤끝 콘솔 `서버 설정 > 인증 정보`에서 Client App ID와 SignatureKey를 복사합니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter1-1.png" />

유니티 상단에 The Backend > Edit Settings 클릭한 후, The Backend Settings 인스펙터 창에 복사한 Client App ID와 Signature Key를 붙여넣습니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter1-2.png" />

### 3. 실행하여 상단 로그인 진행

유니티 실행 버튼을 눌러 해당 씬을 시작합니다. 게임 내 상단 UI에 있는 아이디/비번 칸에 회원가입 혹은 로그인할 아이디를 입력합니다.  

해당 프로젝트에서는 아이디와 비밀번호를 동일한 값으로 입력하고 있으며, 로그인 실패 시 해당 아이디와 비밀번호로 회원가입을 시도하는 로직을 가지고 있습니다.  

로그인이 완료되면 로그인 입력 UI 우측에 inDate,nickname, UID(쿠폰용)이 활성화됩니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter2.png" />

### 4-1. 조회형 함수 테스트

대표적인 조회(불러오기)형 함수로, 게임 운영 관리 > 공지사항 리스트 조회 함수를 클릭합니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter3-1.png" />

불러올 갯수(limit)를 입력합니다. 입력이 완료되었다면 **실행** 버튼을 클릭합니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter3-2.png" />

실행 후에는 결과를 보여주는 UI가 생성됩니다. 해당 UI에는 해당 함수에서 리턴하는 JSON 값들을 파싱하여 보여줍니다.  

### 4-2. 입력형 함수 테스트

대표적인 입력형(생성, 수정) 함수의 예시로 게임 정보 관리 > 게임 정보 삽입을 클릭합니다.  

우측 상단에 `문자 데이터 추가`, `List형 데이터 추가`, `Dictionary형 데이터 추가`중 하나를 클릭하여 데이터를 추가합니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter4-1.png" />

실행 후에는 결과를 보여주는 UI가 생성됩니다. 해당 UI에는 해당 함수에서 리턴하는 JSON 값들을 파싱하여 보여줍니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter4-2.png" />

해당 결과값은 성공한 결과값으로, 생성된 데이터의 고유번호(inDate)를 리턴해줍니다. 생성된 데이터는 뒤끝 콘솔에서 확인이 가능합니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter4-3.png" />

실패한 경우에는 다음과 같이 400 혹은 500번대로 시작하는 statusCode, errorCode, errorMessage가 출력됩니다.  

<img src="https://developer.thebackend.io/static/img/unity/tutorial/new-chapter4-4.png" />
