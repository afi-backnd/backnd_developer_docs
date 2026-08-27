---
sidebar_position: "1"
description: "테이블"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 테이블

## 스키마 유무에 따른 차이

뒤끝에서는 스키마를 정의한 테이블과 정의하지 않은 테이블 두 가지 방식으로 테이블을 생성할 수 있습니다.  
자세한 사항은 [스키마 유무에 따른 차이](/guide/getting-started/how-to-use/game-information)를 참고해 주세요

## Public / Private 테이블

뒤끝은 두 가지 종류의 테이블을 제공합니다. 뒤끝 SDK를 사용하여 정보를 요청할 때 다음과 같은 차이가 있습니다.  

- Public 테이블 : 모든 게이머의 정보를 반환
- Private 테이블 : AccessToken의 게이머 정보만 반환

<table >
    <thead>
        <tr>
            <th rowspan="2" ></th>
            <th className="td-center" colspan="2" >Public 테이블</th>
            <th className="td-center" colspan="2">Private 테이블</th>
        </tr>
        <tr>
            <th className="td-center">읽기</th>
            <th className="td-center">쓰기</th>
            <th className="td-center">읽기</th>
            <th className="td-center">쓰기</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td className="td-center">나</td>
            <td className="td-center">O</td>
            <td className="td-center">O</td>
            <td className="td-center">O</td>
            <td className="td-center">O</td>
        </tr>
        <tr>
            <td className="td-center">타인</td>
            <td className="td-center">O</td>
            <td className="td-center">O</td>
            <td className="td-center">X</td>
            <td className="td-center">X</td>
        </tr>
    </tbody>
</table>

> 쓰기 기능은 게임 정보 생성, 수정, 삭제를 포함합니다.  
> 타인의 public 테이블의 경우 SDK 5.4.0 이상을 사용하는 경우에만 수정할 수 있습니다.  
> SDK 5.4.0 미만에서는 타인의 퍼블릭 테이블을 수정할 수 없습니다.  

## 테이블 관리

테이블 관리는 **뒤끝 콘솔 → 뒤끝베이스 → 게임 정보 관리 → 테이블** 메뉴에서 할 수 있습니다.  
테이블 리스트와 Public/Private 구분, 스키마 기능 여부, 활성/비활성 상태를 확인할 수 있습니다.  
비활성 상태의 테이블은 게임 내(뒤끝 SDK)에서 조회할 수 없습니다.  

테이블 이름을 클릭하여 테이블 내 데이터를 확인할 수 있습니다.  

<ConsoleLinkButton text="테이블 바로가기" menu="baseGameInfo/table" feature="게임 정보" title="테이블" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블-수정.png" />

### 필터링

우측 상단의 셀렉트 박스를 이용해 Public/Private, 스키마 정의/스키마 미정의 테이블을 확인할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블---필터링-수정.png" />

### 검색

우측 상단의 검색창을 이용해 테이블명으로 검색할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블---검색-수정.png" />

## 테이블 생성

페이지 상단의 명령 버튼 중 **테이블 생성** 버튼을 클릭하여 버튼을 생성할 수 있습니다.  

- 테이블은 최대 100개까지 생성 가능합니다.  
- 스키마 정의는 생성 후 변경할 수 없습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블---테이블-생성-버튼.png" />

## 테이블명

- 테이블명은 32자 이하의 '영문 대소문자', '숫자', '\_'의 조합이어야 하며, 시작은 영문자여야 합니다.  
- 테이블명은 생성 후 변경할 수 없습니다.  

### 스키마 미정의 테이블 생성

스키마 없는 테이블을 이용하시려면, 스키마 정의를 **미사용**으로 선택해 주세요.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블---테이블-생성-모달---스키마-미사용.png" />

### 스키마 정의 테이블 생성

스키마 테이블을 이용하시려면 스키마 정의를 **사용**으로 선택하신 뒤, **컬럼 생성**을 해주셔야 합니다.  
컬럼으로 사용하실 수 있는 데이터 타입은 아래와 같습니다.  

- datetime
- string
- list
- map
  - map 타입 컬럼 생성에 사용되는 key파일의 확장자는 xls, xlsx, csv 를 지원합니다.
  - key 파일은 key 제한과 동일한 수의 key 값을 행으로 구분하여 저장해 업로드해야 합니다.
- bool
- int
- float
- double

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블---테이블-생성-모달---스키마-사용---컬럼-생성.png" />

  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블---테이블-생성-모달---스키마-사용---컬럼-생성-모.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블---테이블-생성-모달---스키마-사용---컬럼-생성-모 2.png" />

## 테이블 수정

테이블 우측 수정 아이콘을 통해 테이블을 수정할 수 있습니다.  

- Public / Private 테이블 종류와 테이블 설명, 스키마 정의를 수정할 수 있습니다.  

:::danger 주의
랭킹에 등록되어있는 테이블을 Public으로 변경할 경우, 400 bad public Table에러가 발생하며 갱신되지 않습니다.  
등록된 테이블을 Public으로 변경하는 것은 삼가해주시기 바랍니다.  
:::

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블---테이블-수정.png" />

### 스키마 정의 테이블 수정/삭제

null 허용이 **비허용**일 경우, 해당 컬럼은 삭제할 수 없습니다.  

## 테이블 삭제

페이지 상단의 명령 버튼 중 **삭제** 버튼으로 테이블을 삭제할 수 있습니다.  

- 테이블을 삭제하는 경우, 테이블의 메타데이터만 삭제됩니다.  
테이블 내 데이터 삭제를 원하시는 경우 [데이터 초기화](https://docs.thebackend.io/guide/console-guide/backnd-base/game-information/data#%ED%85%8C%EC%9D%B4%EB%B8%94-%EC%B4%88%EA%B8%B0%ED%99%94) 또는 [데이터 삭제](https://docs.thebackend.io/guide/console-guide/backnd-base/game-information/data#%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%82%AD%EC%A0%9C)기능을 통해 삭제해 주세요.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---테이블-삭제.png" />


## 게임 정보 다운로드
게임 정보를 parquet 파일로 다운로드할 수 있습니다. 다운로드는 데이터 추출, 파일 다운로드 순으로 진행됩니다.

### 데이터 추출하기
계정 비밀번호 및 추출 사유를 입력하면 데이터 추출이 진행됩니다.  
데이터 추출은 비동기로 진행되며, 추출 완료 시 콘솔 우측 상단 알림을 통해 확인할 수 있습니다.  
추출이 진행되는 동안에는 다시 추출할 수 없습니다.

:::info 주요 작업 전, 데이터 추출을 통해 백업본을 확보하세요.  
새로운 업데이트 배포, 게임 밸런스 조정, 대규모 데이터 마이그레이션 등 게임 서비스에 큰 변화가 있는 작업을 앞두고 있다면 '데이터 추출하기'를 실행하여 현재 시점의 데이터를 보관하는 것을 권장합니다.  
사전에 백업된 데이터는 작업 후 예상치 못한 버그나 문제가 발생했을 때, 신속하게 원인을 파악하고 리스크를 최소화하는 데 핵심적인 자료로 활용되어 안정적인 서비스 운영을 돕습니다.  
:::

![게임정보다운로드1](/img/docs/guide/base/game-information/download1.png)

### 추출 결과 다운로드
추출을 요청한 목록을 확인할 수 있으며, 추출 완료된 건을 테이블 별로 다운로드 받을 수 있습니다.  
추출 완료 건을 클릭하면 테이블 목록으로 이동합니다.  
필요한 파일 선택 후 다운로드할 수 있습니다.

![게임정보다운로드2](/img/docs/guide/base/game-information/download2.png)
![게임정보다운로드3](/img/docs/guide/base/game-information/download3.png)

