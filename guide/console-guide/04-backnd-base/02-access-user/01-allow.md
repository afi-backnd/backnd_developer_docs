---
sidebar_label: "접근 허용"
sidebar_position: "1"
description: "유저 접근 관리 - 접근 허용"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 유저 접근 관리 - 접근 허용

뒤끝 콘솔의 유저 접근 관리 메뉴의 **접근 허용** 탭에서는 프로젝트 상태가 점검 상태일 때도 접속이 가능하도록 특정 유저를 등록할 수 있습니다.(화이트리스트)  
등록 가능한 인원은 최대 100명입니다.  

<ConsoleLinkButton text="유저 접근 허용 바로가기" menu="baseGamerAccess/5.11.0/allow" feature="유저 접근" title="유저 접근 관리 - 접근 허용" />

### 검색

검색은 **기간**과 **대상**으로 가능합니다.  

- 검색 기간은 일 단위, 최대 3개월까지 설정할 수 있습니다.  
- **유형/상태** 항목은 선택할 수 없습니다.  
- **대상**에는 유저 번호 또는 닉네임을 입력할 수 있습니다.(멀티 캐릭터 사용 시 유저 번호만 입력할 수 있습니다.)

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근허용-검색.png" />

### 접근 허용 등록

접근을 허용할 유저 정보 및 사유를 입력할 수 있습니다.  

- **대상**에는 유저 번호 또는 닉네임을 입력합니다.(멀티 캐릭터 사용 시 유저 번호만 입력할 수 있습니다.)
- **사유**는 최대 20자 까지 입력 가능하며 선택 입력 항목입니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근허용-등록.png" />

### 접근 허용 등록 불가

접근 허용 등록은 프로젝트 당 100개 까지 가능합니다.  

### 접근 허용 수정

등록된 접근 허용의 **사유** 항목의 수정이 가능합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근허용-수정.png" />

### 접근 허용 유저 삭제

삭제할 유저를 선택(체크박스)한 후 **삭제** 버튼을 누르면 접근 허용 내역을 삭제할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근허용-삭제.png" />

:::tip 멀티 캐릭터를 사용 중인가요?
멀티 캐릭터 계정으로 승격한 경우 기존 접근 허용 유저를 **계정 UUID로 다시 등록**해야 합니다. [보기](/guide/console-guide/backnd-base/users/multi-character#유저-접근)
:::
