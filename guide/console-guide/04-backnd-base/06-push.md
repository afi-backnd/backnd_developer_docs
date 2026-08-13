---
description: "푸시 알림"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 푸시 알림

뒤끝 콘솔 **푸시 알림**에서는 푸시를 등록하여 예약된 시간에 로그인된 기기로 푸시 알람을 보낼 수 있습니다.

:::danger 주의
콘솔에서 푸시 발송 기능을 이용하려면 먼저 서버 설정에서 푸시 정보 등록이 필요합니다.  
서버 설정 → 푸시 → 푸시 등록과 클라이언트에서의 기능에 대해서는 아래 링크를 참고해 주세요.  
Android(구글, 원스토어) 푸시 등록 : <a href="/sdk-docs/backend/base/push/android/active" target="_blank">개발자 문서 - Android 푸시 등록</a>  
애플 푸시 등록 : <a href="/sdk-docs/backend/base/push/ios/active" target="_blank">개발자 문서 - iOS 푸시 등록</a>  
멀티 캐릭터 사용 시에도 한번만 푸시가 전송이 됩니다.  
:::

### 일반 푸시

<ConsoleLinkButton text="일반 푸시 바로가기" menu="basePush/push" feature="푸시 알림" title="푸시 알림" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---일반-푸시.png" />

### 반복 푸시

<ConsoleLinkButton text="반복 푸시 바로가기" menu="basePush/repeat" feature="푸시 알림" title="푸시 알림" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---반복-푸시.png" />

## 푸시 등록

페이지 상단의 명령 버튼 중 **푸시 등록** 버튼을 클릭해 새로운 푸시를 등록할 수 있습니다.

- 발송일은 즉시 발송, 예약 발송, 반복 발송 중 선택 가능합니다.
- 입력창 상단에 있는 **+** 버튼을 클릭하여 국가에 따라 다른 내용을 작성할 수 있습니다.
- 국가 추가는 유저가 한 명이라도 국가 코드를 가지고 있어야 가능하며, 유저들이 가지고 있는 국가만 노출됩니다.
- 클라이언트에서 Device Token이 활성화되지 않은 유저는 푸시 알람이 보내지지 않습니다.
- 동일한 시간 내에 푸시가 여러개 있는 경우, 해당 푸시중 일부는 기기로 오지 않을 수 있습니다.

### 발송 대상

발송대상은 **인증정보** 메뉴에서 플레이스토어와 원스토어에 같은 패키지 이름이 적용되었는지에 따라 달라집니다.<br/> 같은 패키지 이름이 적용된 경우, Android (Google Play Store), Android (ONE Store), iOS(Development), iOS(Production)<br/> 다른 패키지 이름이 적용된 경우, Android, iOS(Development), iOS(Production)

### 즉시발송

등록 즉시 푸시가 발송됩니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-등록---즉시-발송.png" />

### 예약발송

예약 발송 시 가장 가까운 30분 뒤부터 30분 단위로 선택이 가능하며 과거 시간은 선택할 수 없습니다.(최대 30일 후까지 가능)
<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-등록---예약-발송.png" />

### 반복발송

매일, 매주, 매월 특정일시에 푸시를 반복적으로 발송할 수 있습니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-등록---반복-발송.png" />

### 국가추가

푸시 내용 입력 칸 상단에 + 버튼을 눌러 다른 국가도 추가/제거할 수 있습니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-등록---국가-추가.png" />

## 푸시 수정

해당 푸시를 클릭하여 등록된 푸시를 수정할 수 있습니다.

- 발송 완료된 푸시는 수정하실 수 없습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-수정.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-수정-모달.png" />

반복 푸시의 경우, 발송된 푸시의 내역을 확인할 수 있습니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---반복-푸시-내역.png" />

우측에 **반복 종료** 버튼을 통해 발송된 푸시 내역은 남기고 반복을 종료할 수 있습니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---반복-푸시-종료.png" />

## 푸시 테스트

푸시 테스트는 유저 한 명의 닉네임을 검색하여 푸시를 즉시 발송하는 기능입니다.

- Device Token이 활성화되지 않은 유저에겐 푸시 알람이 보내지지 않습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-테스트.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-테스트-모달.png" />

## 푸시 삭제

페이지 상단의 명령 버튼 중 **삭제** 버튼을 클릭해 푸시를 삭제할 수 있습니다.

- 이미 발송된 푸시는 삭제가 불가합니다.

### 일반 푸시

<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---푸시-삭제.png" />

### 반복 푸시

반복 푸시는 삭제할 경우, 반복이 종료됩니다.

## 우편 기능의 푸시

뒤끝 우편 기능을 이용하여 우편 등록 시, 우편 발송시에 함께 발송되는 푸시를 등록할 수 있습니다.  
해당 우편 등록 시, 푸시 알림에도 자동으로 우편에 등록된 푸시가 등록됩니다.(반복 우편은 반복 푸시 메뉴에 등록됩니다.)

- 예약 우편 및 반복 우편과 함께 등록된 푸시는 발송 이전 삭제가 가능하며, 푸시 삭제 후 우편 발송 시 우편만 발송이 되고 푸시는 발송되지 않습니다.

### 우편 기능

<img src="https://developer.thebackend.io/static/img/newconsole/base/푸시관리/뒤끝베이스--푸시-관리---우편-푸시-발송.png" />
