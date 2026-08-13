---
description: "이벤트(구버전)"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 이벤트(구버전)

:::warning 구버전 안내
해당 페이지는 구버전 이벤트 기능에 대한 문서입니다.  
최신 버전 문서는 **[이벤트](/guide/console-guide/backnd-base/event/event)**을 참고해 주세요.
:::

**이벤트**를 등록/수정 혹은 삭제하실 수 있습니다.  
SDK에서의 함수 호출은 [개발자 문서-이벤트 조회하기](/sdk-docs/backend/base/operation/event/get-list)를 참고해 주세요.  

<ConsoleLinkButton text="이벤트 바로가기" menu="baseEvent" feature="이벤트" title="이벤트" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/이벤트/뒤끝베이스-이벤트.png" />

## 이벤트 등록

**이벤트 등록** 버튼을 눌러 새로운 이벤트를 등록할 수 있습니다.   

<img src="https://developer.thebackend.io/static/img/newconsole/base/이벤트/뒤끝베이스-이벤트-이벤트등록버튼.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/이벤트/뒤끝베이스-이벤트-이벤트등록-모달.png" />

### 작성자

- 글 작성 시 로그인되어 있는 계정의 닉네임

### 게시 일시

- 즉시 게시 : 등록 즉시 게시됩니다.  
- 예약 게시 : 30분 단위로 설정 가능하며 현재 시간 이전으로 설정이 불가합니다.  

### 이벤트 기간

- 기간은 **하루, 7일, 30일, 직접 입력**이 가능하며 직접 입력 시, 30분 단위로 입력이 가능합니다.  

### 국가 추가

이벤트를 국가별로 다른 내용(다른 언어)을 지정하여 노출시킬 수 있습니다.  

- 이벤트 입력 칸 상단의 **+버튼**을 누르시면 해당 국가 코드의 유저에게 노출시킬 내용 입력 칸이 생성됩니다.  
- 국가 선택에 **국가 코드는 현재 유저들에게 설정되어 있는 국가를 기준으로 표시**됩니다.  
  - 이벤트 내용을 다르게 노출시키고 싶은 국가 코드를 지닌 유저가 생성되어 있어야 해당 국가 코드가 노출됩니다.  
- 국가 지정은 최대 5개까지 지정할 수 있습니다.  
- 국가 코드를 지니지 않은 유저나 이벤트에 국가 설정이 지정되지 않은 유저에게는 **기본** 탭의 내용이 노출됩니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/이벤트/뒤끝베이스-이벤트-이벤트등록-모달---국가추가.png" />

### 제목

- 필수 항목이며 최대 60자까지 입력할 수 있습니다.  

### 내용

- 필수 항목이며 최대 4,000자까지 입력할 수 있습니다.  

### 컨텐츠 이미지 첨부

- 첨부 시, 내용보다 위에 삽입되며 한 장만 첨부 가능합니다.(최대 1MB)

### 팝업 이미지 첨부

- 이벤트 본문이 아닌 게임 접속 시 바로 띄우는 팝업용 이미지이며 최대 1MB입니다.  

### 외부 링크 버튼

- 내용 아래에 입력한 버튼 명으로 버튼이 생성되며, 클릭 시 입력한 URL로 이동됩니다.  
- 버튼 이름 항목은 30자까지, URL 생성 항목은 200자까지 입력할 수 있습니다.  

### 공개 여부 설정

- 비공개 설정 시 노출되지 않습니다.  

## 이벤트 수정

수정 버튼을 클릭하여 이벤트에 등록된 정보들을 수정할 수 있습니다.   

<img src="https://developer.thebackend.io/static/img/newconsole/base/이벤트/뒤끝베이스-이벤트-이벤트-편집.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/이벤트/뒤끝베이스-이벤트-이벤트-편집-모달.png" />

## 이벤트 삭제

페이지 상단의 명령 버튼 중 **삭제** 버튼을 눌러 이벤트를 삭제할 수 있습니다.   

<img src="https://developer.thebackend.io/static/img/newconsole/base/이벤트/뒤끝베이스-이벤트-이벤트-삭제.png" />
