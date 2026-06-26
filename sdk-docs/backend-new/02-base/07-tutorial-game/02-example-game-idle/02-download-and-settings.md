---
sidebar_label: 다운로드 및 게임 설정
description: "다운로드 및 게임 설정"
sidebar_position: 2
---

# 다운로드 및 게임 설정

'BackendHero : 방치형'게임을 정상적으로 실행하기 위해서는 다음과 같은 설정들이 필요합니다.  

- 유니티 - 씬 설정
- 유니티 - 뒤끝에 clientAppId, signatureKey 값 설정
- 뒤끝 콘솔 - 게임 정보 테이블 5개 생성
- 뒤끝 콘솔 - 차트 7개 생성 및 차트 다운받아 적용

다음은 게임 실행 이후에 설정해야하는 항목입니다.  

- 뒤끝 콘솔 - 랭킹
- 뒤끝 콘솔 - 우편

## 유니티 설정

### 1. 상단 유니티 패키지를 다운로드합니다.  

- [BackendIdle.unitypackage](https://developer.thebackend.io/sdk/script/tutorial/5.11.1/BackendIdle.unitypackage)
  > 해당 예제는 Unity 2021.3.6f1과 Backend-5.8.0-dotnet4버전을 기준으로 개발되었습니다.  

### 2. 유니티 빈 프로젝트에 해당 패키지를 적용합니다.  

### 3. 뒤끝 콘솔에서 새 프로젝트를 생성합니다.  

- 해당 프로젝트에서 Client App ID와 Signature Key를 발급받습니다. [해당 문서](/sdk-docs/backend/base/start-up)를 참고하시면 됩니다.  
- 뒤끝 SDK의 경우 이미 소스코드에 포함되어 있습니다. 별도로 설치하지 않으셔도 됩니다.  

### 4. 3번 항목에서 발급받은 Client App ID와 Signature Key를 The Backend > Edit Settings에 입력합니다.  

<img src="https://developer.thebackend.io/static/img/unity/idleGame/sdkSetting.png" />

### 5. 유니티 상단 File 탭 클릭 > Build Settings > Scenes in Build에 다음 순서로 씬을 등록합니다.  

씬은 Assets > Scenes에서 확인할 수 있습니다.  

1. LoginScene
2. LoadingScene
3. InGameScene

<img src="https://developer.thebackend.io/static/img/unity/idleGame/sceneSetting.png" />

---

## 콘솔 설정

### 1. 뒤끝 콘솔 > 게임 정보 관리에서 **Private / 스키마 미정의 테이블**로 다음 테이블들을 생성합니다.  

- QuestAchievement
- UserData
- ItemInventory
- WeaponEquip
- WeaponInventory

<img src="https://developer.thebackend.io/static/img/unity/idleGame/gameData2.png" />
<img src="https://developer.thebackend.io/static/img/unity/idleGame/gameData1.png" />

### 2. 뒤끝 콘솔 > 차트 관리에서 해당 차트명의 차트들을 생성합니다.(3번에 차트명 기재)

- forPost
- weaponChart
- itemChart
- questChart
- shopChart
- stageChart
- enemyChart

<img src="https://developer.thebackend.io/static/img/unity/idleGame/chart1.png" />

### 3. 차트들을 다운로드 받습니다.  

<table>
  <tr>
    <th>차트 명</th>
    <th>파일</th>
  </tr>
  <tr>
    <td>forPost</td>
    <td><a href = "https://developer.thebackend.io/files/idleGame/forPost.csv">forPost.csv</a></td>
  </tr>
  <tr>
    <td>weaponChart</td>
    <td><a href = "https://developer.thebackend.io/files/idleGame/weaponChart.csv">weaponChart.csv</a></td>
  </tr>
  <tr>
    <td>itemChart</td>
    <td><a href = "https://developer.thebackend.io/files/idleGame/itemChart.csv">itemChart.csv</a></td>
  </tr>
  <tr>
    <td>questChart</td>
    <td><a href = "https://developer.thebackend.io/files/idleGame/questChart.csv">questChart.csv</a></td>
  </tr>
  <tr>
    <td>shopChart</td>
    <td><a href = "https://developer.thebackend.io/files/idleGame/shopChart.csv">shopChart.csv</a></td>
  </tr>
  <tr>
    <td>stageChart</td>
    <td><a href = "https://developer.thebackend.io/files/idleGame/stageChart.csv">stageChart.csv</a></td>
  </tr>
  <tr>
    <td>enemyChart</td>
    <td><a href = "https://developer.thebackend.io/files/idleGame/enemyChart.csv">enemyChart.csv</a></td>
  </tr>
</table>

### 4. 3에서 다운받은 차트들을 차트명과 동일한 차트에 업로드 한 후, 해당 차트를 선택하여 '차트 적용' 버튼을 클릭합니다.  

<img src="https://developer.thebackend.io/static/img/unity/idleGame/chart3.png" />
<img src="https://developer.thebackend.io/static/img/unity/idleGame/chart4.png" />
<img src="https://developer.thebackend.io/static/img/unity/idleGame/chart2.png" />

### 4. 게임 시작

이제 예제 게임을 실행한 준비는 끝났습니다.  
유니티에서 LoginScene으로 이동하여 게임을 실행하세요.  
랭킹의 경우, 게임 시작을 통해 게임 정보 테이블에 데이터가 생성된 후 이용할 수 있습니다.  

이후 랭킹 및 우편 설정 방법은 [랭킹, 우편 사용 방법](/sdk-docs/backend/base/tutorial-game/rank-and-post)을 참고해주세요.  
