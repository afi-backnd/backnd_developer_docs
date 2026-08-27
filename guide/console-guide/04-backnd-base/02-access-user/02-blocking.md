---
sidebar_label: "접근 차단"
sidebar_position: "2"
description: "유저 접근 관리 - 접근 차단"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 유저 접근 관리 - 접근 차단

뒤끝 콘솔의 유저 접근 관리 메뉴의 **접근 차단** 탭에서는 유저 또는 디바이스 별 접근 차단 설정이 가능합니다.  

<ConsoleLinkButton text="유저 접근 차단 바로가기" menu="baseGamerAccess/5.11.0/deny" feature="유저 접근" title="유저 접근 관리 - 접근 차단" />

## 검색

검색은 **기간**, **유형/상태**, **대상**으로 가능합니다.  

- **검색 기간**은 일 단위, 최대 3개월까지 설정할 수 있습니다.  
- **유형** 항목은 유저 또는 디바이스를 선택할 수 있습니다.  
- **상태** 항목은 전체/차단/해제 상태를 선택할 수 있습니다.  
- **대상**에는 유저 번호 또는 닉네임을 입력할 수 있습니다.(멀티 캐릭터 사용 시 유저 번호만 입력할 수 있습니다.)

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-검색.png" />

## 유저 검색

유형 **유저**로 검색 시 접근 차단된 유저 목록을 확인할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-유저검색.png" />

- 디바이스 정보
  디바이스 정보를 2개 이상 보유하고 있는 유저의 경우 **디바이스 정보** 열의 "더 보기" 아이콘을 누르면 상세 내역을 확인할 수 있습니다.  
  > <img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-유저디바이스목록1.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-유저디바이스목록2.png" />

## 디바이스 검색

유형 **디바이스**로 검색 시 접근 차단된 디바이스 목록을 확인할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-디바이스검색.png" />

## 접근 차단 등록 - 유저

접근을 차단할 유저와 기간 및 사유를 입력합니다.

- 유저는 **직접 입력** 또는 **CSV 업로드** 방식으로 등록할 수 있습니다.
    - 직접 입력은 UUID 또는 닉네임을 콤마(,)로 구분하여 **최대 10명**까지 입력 가능합니다.
    - 멀티 캐릭터 사용 시, 유저 UUID만 입력 가능합니다.
    - 이미 등록된 유저나 존재하지 않는 유저는 등록할 수 없습니다.
- ‘차단 시 리더보드 순위 삭제하기’ 옵션을 선택하면, 차단과 동시에 해당 유저의 리더보드 순위가 삭제됩니다.
- **사유**는 선택 입력 항목이며, 최대 100자까지 입력 가능합니다.

![user block](/img/docs/guide/base/access-user-blocking/user-block.png)

### 유저 CSV 업로드

![user block](/img/docs/guide/base/access-user-blocking/csv-upload.png)

[예시 파일](/files/banList_example.csv)과 같이 유저 UUID를 입력하여 업로드하세요.

![user block](/img/docs/guide/base/access-user-blocking/csv-upload-example.png)
- 열(column) 이름은 반드시 **‘uuid’**로 설정해야 합니다.
- 각 행에는 하나의 유저 UUID만 입력할 수 있으며, 한 번에 최대 500개의 UUID를 입력할 수 있습니다.
- .csv, .xls, .xlsx 파일 모두 업로드 가능합니다. 단, 업로드 파일은 반드시 **단일 시트(sheet)**로 구성되어야 합니다.

### 접근 차단 등록 처리 결과

- 실패한 항목이 있을 경우, 실패 사유와 목록이 표시됩니다.
- CSV 업로드의 처리 결과에서는 ‘실패 목록 CSV 다운로드’를 통해 실패한 항목과 사유가 포함된 파일을 다운로드할 수 있습니다.
  - 다운로드한 파일을 그대로 다시 업로드하면 ‘실패 사유’ 컬럼은 무시되며, ‘uuid’ 컬럼만 처리됩니다.

![user block](/img/docs/guide/base/access-user-blocking/csv-upload-result.png)

:::tip 멀티 캐릭터를 사용 중인가요?
멀티 캐릭터 계정으로 승격한 경우 기존 접근 차단은 **계정 UUID로 자동 변경**됩니다. [보기](/guide/console-guide/backnd-base/users/multi-character#유저-접근)
:::

## 접근 차단 등록 - 디바이스

접근을 차단할 디바이스 정보 및 사유를 입력합니다.
(SDK 5.9.0 이상으로 로그인한 유저의 디바이스만 등록 가능합니다.)

- 디바이스 차단을 등록할 **유저**를 검색 합니다.
    - UUID 또는 닉네임을 입력할 수 있으며 필수 입력 항목입니다.
    - 멀티 캐릭터 사용 시 UUID만 입력할 수 있습니다.
- 유저를 검색하면 **디바이스** 항목에서 차단할 디바이스를 선택할 수 있습니다.  
  - 이미 등록된 디바이스는 선택할 수 없으며 필수 선택 항목입니다.
- **사유**는 선택 입력 항목이며, 최대 100자까지 입력 가능합니다.

![device block](/img/docs/guide/base/access-user-blocking/device-block.png)

## 접근 차단 수정

등록된 접근 차단의 **사유** 항목의 수정이 가능합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-수정.png" />

## 접근 차단 유저/디바이스 해제

해제할 유저/디바이스를 선택(체크박스)한 후 **접근 차단 해제** 버튼을 누르면 접근 차단을 해제할 수 있습니다.(차단-> 해제 이력이 유지됩니다.)

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-해제.png" />

## 접근 차단 목록 CSV 다운로드

검색 기간을 설정하여 결과가 있는 경우에 차단 등록된 목록을 CSV로 다운로드할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-csv.png" />

## 접근 차단 유저/디바이스 삭제

삭제할 유저/디바이스를 선택(체크박스)한 후 **삭제** 버튼을 누르면 접근 차단 내역을 삭제할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 접근 관리/뒤끝베이스-유저접근관리-접근차단-삭제.png" />
