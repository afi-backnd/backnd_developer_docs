---
sidebar_label: "랭킹, 우편 설정 방법"
sidebar_position: "3"
description: "랭킹, 우편 설정 방법"
---

# 랭킹, 우편 설정 방법

### 게임 시작

랭킹 항목의 테이블과 컬럼이 활성화되려면 게임이 플레이되고 데이터가 생성되야합니다.  
따라서 게임 시작을 1회 한 후, 랭킹을 생성해주시기 바랍니다.  

### 랭킹 생성

아래 화면과 같이 랭킹이 존재하지 않는다는 UI가 발생할 경우, 뒤끝 콘솔 > 랭킹에서 랭킹을 생성합니다.  

<img src="https://developer.thebackend.io/static/img/unity/idleGame/rank1.png" />

- 유형 : 유저 랭킹
- 랭킹 명 : 레벨
- 초기화 기간 : 누적랭킹
- 테이블 : UserData
- 컬럼 : Level
- 추가항목 : 없음
- 정렬 기준 : 내림차순

## 우편 발송

우편을 발송하고자 할 경우, 차트 관리에서 일부 차트의 우편 기능 여부를 '사용'으로 변경해야합니다.  

### 차트 관리 > 우측 편집 버튼을 눌러 우편 기능을 사용으로 바꿉니다.  

발송 가능한 우편은 'forPost', 'weaponChart', 'itemChart' 입니다.  

<img src="https://developer.thebackend.io/static/img/unity/idleGame/post1.png" />

### 우편 관리 > 우편 생성

우편관리에서 우편 생성을 클릭하고 아이템 추가를 통해 등록한 차트 정보를 이용하여 우편 보상을 등록합니다.  
게임 내에서는 발송과 동시에 보여지지 않으며, 게임을 재접속 하시는 것을 추천드립니다.  

<img src="https://developer.thebackend.io/static/img/unity/idleGame/post2.png" />
<img src="https://developer.thebackend.io/static/img/unity/idleGame/post3.png" />
