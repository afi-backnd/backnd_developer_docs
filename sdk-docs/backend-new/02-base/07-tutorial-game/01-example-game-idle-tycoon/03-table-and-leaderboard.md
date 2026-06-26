---
sidebar_label: "데이터 테이블 및 리더보드 설정"
sidebar_position: 3
---

# 데이터 테이블 및 리더보드 설정

## 뒤끝 콘솔에서 게임 테이블 및 리더보드 설정

게임 내에서 플레이어의 진행 상황, 수집 아이템, 로그인 기록 등을 저장하고 불러오기 위해 뒤끝 콘솔에 총 4개의 사용자 데이터 테이블과 1개의 리더보드를 생성해야 합니다.

생성해야할 테이블 4개는 아래와 같습니다.  
- UserWardrobeData  
- UserStageData  
- UserMainData   
- UserDailyLoginData  

아래의 내용을 참고하여 테이블을 생성해 주세요.  

:::tip
> ✅ *스키마 미사용 테이블*은 자유롭게 데이터를 저장할 수 있으며 컬럼 구조를 고정하지 않아도 됩니다.  
> ✅ *스키마 사용 테이블*은 정해진 컬럼명과 타입이 필요하며, 저장 시 이 구조를 반드시 따라야 합니다.
:::

### 1. 뒤끝 콘솔 > 게임 정보 관리에서 [Private / 스키마 미사용] 테이블로 다음 테이블들을 생성합니다.  

- 테이블 이름 : UserWardrobeData
  - 테이블 설명 : User Dress Data

하단 이미지와 같은 설정**(Private / 스키마 미사용)**으로 테이블을 생성해주세요.

![image](/img/docs/guide/base/example-games/idle-tycoon/tableWardrobe.png)

### 2. 뒤끝 콘솔 > 게임 정보 관리에서 [Private / 스키마 사용] 테이블로 다음 테이블들을 생성합니다.  

|테이블이름         | 테이블 설명      |
|------------------|-----------------|
|UserStageData     |Player Stage Data|
|UserMainData      |Player Data      |
|UserDailyLoginData|Player Login Data|

하단 이미지와 같이 설정으로**(Private / 스키마 사용)** 테이블을 생성해주세요.  
스키마 정의 테이블이기 때문에 **컬럼명과 데이터 타입을 반드시 지정**해야합니다.  
생성해야하는 스키마 정의 테이블은 총 3개 입니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/tableStageData.png)

생성한 각 테이블 내부 컬럼의 이름과 데이터 타입은 아래와 같습니다.  

#### UserStageData 테이블
 
 |이름         | 데이터 타입 | 기본 값 |null  |  
 |-------------|------------|--------|-------| 
 |StageID      |string      |        |허용   |  
 |PlayerCoin   |double      | 0      |허용   |  
 |KitchenDatas |string      | []     |허용   |  
 |UpgradeLevel |string      | []     |허용   | 

#### UserMainData 테이블
 
 |이름                | 데이터 타입 | 기본 값 |null  |  
 |-------------------|------------|--------|-------|  
 |Gems               |double      | 0      |허용   |  
 |UsedDressID        |string      |        |허용   |  
 |TotalCoinsEarned   |double      | 0      |허용   |  
 |UsedProfileImageID |string      |        |허용   |   

#### UserDailyLoginData 테이블
 
 |이름                       | 데이터 타입 | 기본 값 |null  |  
 |---------------------------|------------|--------|-------| 
 |DailyRewardIdx             |int         | 0      |허용   |  
 |TotalLoginCount            |int         | 0      |허용   |  
 |LoginStreakCount           |int         | 0      |허용   |  
 |LastClaimedDailyRewardDate |string      |        |허용   | 


:::info
컬럼 이름과 타입은 대소문자까지 정확히 입력해야 하며, 오타가 있으면 데이터 저장 시 오류가 발생합니다.  
:::

### 3. 뒤끝 콘솔 > 리더 보드에서 **리더보드 생성하기**를 눌러 리더보드를 생성합니다.  

리더보드 생성 시 아래의 내용과 같이 설정하여 리더보드를 생성해 주세요.

 - 리더보드 이름  : PlayerTotalCoins  
 - 그룹별 구분여 여부 : 구분 안 함  
 - 집계 대상 : 유저  
 - 초기화 주기/시각  : 없음  
 - 집계 필드 : UserMainData > TotalCoinEarned  
 - 정렬 : 내림 차순

:::info
집계 필드 : 랭킹 기준이 되는 숫자 데이터 필드입니다. 예: 총 코인 수, 점수 등  
UUID: 리더보드의 고유 식별자입니다. Unity 프로젝트와 연동할 때 필요합니다.  
:::

아래의 이미지를 참고하여 리더보드를 생성합니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/createLeaderBoard-1.png)
![image](/img/docs/guide/base/example-games/idle-tycoon/createLeaderBoard-2.png)


### 4. 유니티 프로젝트에 리더보드 설정

Unity에서 리더보드를 연동하려면, 방금 생성한 리더보드의 **UUID** 값을 입력해야 합니다.  
**리더보드의 UUID를 유티니 프로젝트에 반드시 입력**해야 리더보드가 정상 작동합니다.  
  
리더보드의 UUID를 복사합니다.

![image](/img/docs/guide/base/example-games/idle-tycoon/copyLeaderBoardUUID.png)

복사한 리더보드 UUID를 입력하기 위해서 **Gameplay씬으로 이동**해야합니다.

1. **뒤끝 콘솔**에서 리더보드 UUID를 복사합니다.  
2. Unity 에디터에서 `Assets/Scenes/Gameplay.unity` 씬을 엽니다.  
2. 씬이 열리면 Hierarchy에서 `Game Managers > LeaderboardCoinsAccumulation` 오브젝트를 선택합니다
3. `LeaderboardCoinsAccumulation`의  Inspector 창에서 `Leaderboard ID` 필드에 복사한 UUID를 붙여넣습니다.

아래의 이미지를 참고하여 세팅을 해주시기 바랍니다.

![image](/img/docs/guide/base/example-games/idle-tycoon/pasteLeaderBoardUUID.png)
