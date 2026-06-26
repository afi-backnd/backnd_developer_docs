---
sidebar_position: 2
---
import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 데이터

## 데이터 관리

데이터 관리는 **뒤끝 콘솔 → 뒤끝베이스 → 게임 정보 관리 → 데이터** 메뉴에서 할 수 있습니다.  

<ConsoleLinkButton text="데이터 바로가기" menu="baseGameInfo/data" feature="게임 정보" title="데이터" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터.png" />

## 테이블 선택

테이블 선택 박스를 클릭하면 테이블 이름으로 검색할 수 있습니다.(대문자와 소문자의 구별이 필요합니다.)  
또한 해당 박스 상단에 위치한 정렬 버튼을 통해 테이블 이름으로 내림차순, 오름차순 정렬이 가능합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터---테이블-검색.png" />

## 데이터 검색

상세 검색 기능을 이용해 해당 테이블에 삽입된 데이터를 검색할 수 있습니다.  

- 대상 : 유저 번호(gamer_id), 유저 아이디, 닉네임을 통해 검색 가능합니다.(멀티 캐릭터 사용 시 캐릭터 번호와 닉네임으로 검색할 수 있습니다.)
- 기간 : inDate(삽입된 시간), updatedAt(갱신된 시간)을 통해 검색 가능합니다.  
- 속성 : column 명, 해당 데이터의 데이터 타입(String, Number 또는 Boolean형), 데이터 비교(=, <=, >=, Between 등, Boolean의 경우 =, ≠, Exists, Not exists), 비교 대상을 통해 검색 가능합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/게임정보관리_데이터_검색_업데이트.png" />

## 데이터 생성

페이지 상단의 명령 버튼 중 **행 생성**버튼을 클릭한 후 gamer_id(유저 번호. 멀티 캐릭터 사용 시 캐릭터 번호)를 입력해 새로운 행(row)을 추가할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터---행-생성-버튼.png" />

  


- 스키마 테이블의 경우, 컬럼(Column) 추가가 불가능합니다.  

  > <img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터---행-생성-모달-(스키마-테이블).png" />

- 비 스키마 테이블의 경우, **컬럼 생성** 버튼으로 해당 로우(row)에 컬럼(Column) 및 값을 추가할 수 있습니다.  
  > <img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터---행-생성-모달.png" />

## 데이터 수정

해당 로우(row)에서 수정할 컬럼을 더블 클릭한 뒤, 값을 수정하고 확인 버튼을 눌러 데이터를 수정할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터---데이터-수정-모달.png" />

## 컬럼 추가

스키마 미정의 테이블의 경우 수정 화면에서 컬럼을 추가할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터---컬럼-추가.png" />

  


<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터----컬럼-추가-or-삭제.png" />

## 데이터 삭제

삭제할 로우(row)를 선택한 후 페이지 상단의 명령 버튼 중 **삭제** 버튼을 클릭해 해당 로우(row)를 삭제할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터----데이터-삭제.png" />

## 테이블 초기화

페이지 상단의 명령 버튼 중 **테이블 초기화**버튼을 클릭해 해당 테이블의 데이터를 초기화할 수 있습니다.  
해당 기능은 **서버 설정 → 프로젝트 → 출시 설정이 모두 테스트**로 설정되어 있을 때 사용 가능합니다.  
초기화할 경우, 해당 테이블의 모든 데이터는 삭제되며 복구는 불가능합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터----테이블-초기화-버튼.png" />

  


<img src="https://developer.thebackend.io/static/img/newconsole/base/게임 정보 관리/뒤끝베이스--게임-정보-관리---데이터----테이블-초기화-버튼-복사.png" />
