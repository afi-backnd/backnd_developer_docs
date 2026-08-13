---
sidebar_position: "1"
description: "로그 관리"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 로그 관리

뒤끝 콘솔 **로그 관리**에서는 클라이언트에서 뒤끝 함수 [InsertLog](/sdk-docs/backend/base/game-log/insert)로 생성된 로그를 확인할 수 있습니다.  
사용한 InsertLog의 버전을 선택하여 해당 로그를 상세하게 확인할 수 있습니다.  

- 3개월이 경과된 로그는 조회할 수 없습니다.  

<ConsoleLinkButton text="로그 바로가기" menu="baseLog/log" feature="로그" title="로그 관리" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/%EC%9C%A0%EC%A0%80%20%EA%B4%80%EB%A6%AC/image%2063.png" />

## 검색

유저 번호(멀티 캐릭터 사용 시 캐릭터 번호)나 닉네임, 기간 등을 입력하여 필요한 로그를 검색할 수 있습니다.  
행동 유형 중 로그인 성공(thebackend_login_success_log)과 로그인 실패(thebackend_login_fail_log)는 기본으로 제공됩니다.  

### 행동 유형 : 커스텀

클라이언트에서 **InsertLogV2** 또는 **InsertLog** 호출 시, **행동 유형** 항목에 첫 번째 인자 값 **string logType**에 삽입된 데이터가 자동 생성됩니다.  
커스텀 로그는 행동 유형 선택 시, 디폴트인 전체 기간으로 정렬됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/%E1%84%85%E1%85%A9%E1%84%80%E1%85%B3%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5/%EB%92%A4%EB%81%9D%EB%B2%A0%EC%9D%B4%EC%8A%A4--%EB%A1%9C%EA%B7%B8-%EA%B4%80%EB%A6%AC-%EC%BB%A4%EC%8A%A4%ED%85%80%EB%A1%9C%EA%B7%B8_%EA%B8%B0%EB%B3%B8.png" />

### 행동 유형 : thebackend_login_success_log

행동 유형이 thebackend_login_success_log일 경우, 검색 설정이 다음과 같이 변경됩니다.  

- 하루 단위로 만 선택 가능(기간 검색 불가)
- 분 단위까지 시간 선택 가능(예 2020.03.26 0:28 - 15:43)

<img src="https://developer.thebackend.io/static/img/newconsole/base/%E1%84%85%E1%85%A9%E1%84%80%E1%85%B3%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5/%EB%A1%9C%EA%B7%B8%EC%9D%B8%EB%A1%9C%EA%B7%B8_%EA%B8%B0%EB%B3%B8.png" />

### 행동 유형 : thebackend_login_fail_log

행동 유형이 thebackend_login_fail_log일 경우, 검색 설정이 다음과 같이 변경됩니다.  

- 하루 단위로 만 선택 가능(기간 검색 불가)
- 분 단위까지 시간 선택 가능(예 2020.03.26 0:28 - 15:43)

또한 출력 영역이 다음과 같이 변경됩니다.  

- 유저(캐릭터) 번호/닉네임 항목 대신 IP 항목이 대체(유저(캐릭터) 번호, 닉네임 확인이 불가능하므로 IP로 식별)
- 유저 관리 버튼 없음

<img src="https://developer.thebackend.io/static/img/newconsole/base/%E1%84%85%E1%85%A9%E1%84%80%E1%85%B3%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5/%EB%A1%9C%EA%B7%B8%EC%9D%B8%EB%A1%9C%EA%B7%B8_%EC%8B%A4%ED%8C%A8.png" />

### 행동 유형 : 애플 계정 변경 웹훅 수신 로그

Apple 계정 변경 로그는 아래 행동 유형으로 저장됩니다.

- **Apple_email_enabled**: 프로젝트에 대해 해당 유저의 가리기 이메일(Private Relay) 수신이 활성화된 경우
- **Apple_email_disabled**: 프로젝트에 대해 해당 유저의 가리기 이메일(Private Relay) 수신이 비활성화된 경우
- **Apple_consent_revoked**: 프로젝트에 대해 해당 유저의 Apple 로그인 연동이 해제된 경우
- **Apple_account_delete**: 해당 유저의 Apple 계정이 삭제된 경우

:::info
애플 계정 웹훅과 관련된 자세한 정보는 [인증정보 - Apple 계정 변경 로깅](/guide/console-guide/server-setting/authenciation#apple-계정-변경-로깅)을 참고해 주세요.
:::

## 로그 내역 조회

로그의 행을 클릭하면 해당 로그 내용을 볼 수 있는 아코디언 UI가 활성화 됩니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/%E1%84%85%E1%85%A9%E1%84%80%E1%85%B3%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5/%EB%A1%9C%EA%B7%B8%EC%9D%B8%EB%A1%9C%EA%B7%B8_%EC%8B%A4%ED%8C%A8.png" />

## 유저 관리

로그 행 우측의 **유저 관리** 버튼을 누르면 해당 로그를 보낸 유저의 정보를 확인할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/%E1%84%85%E1%85%A9%E1%84%80%E1%85%B3%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5/%EB%A1%9C%EA%B7%B8_%EB%A1%9C%EC%9A%B0_%EC%9C%A0%EC%A0%80%EA%B4%80%EB%A6%AC_%EC%95%84%EC%9D%B4%EC%BD%98.png" />
<img src="https://developer.thebackend.io/static/img/newconsole/base/%E1%84%85%E1%85%A9%E1%84%80%E1%85%B3%E1%84%80%E1%85%AA%E1%86%AB%E1%84%85%E1%85%B5/%EA%B2%8C%EC%9D%B4%EB%A8%B8%EC%A0%95%EB%B3%B4_1.png" />
