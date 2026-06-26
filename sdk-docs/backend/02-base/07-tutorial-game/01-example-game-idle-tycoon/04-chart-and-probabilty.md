---
sidebar_label: 차트 및 확률 설정
sidebar_position: 4
---

# 차트 및 확률 설정

## 차트/확률 생성 및 파일 업로드

게임 플레이에는 다양한 밸런스 수치, 아이템 구성, 보상 정보 등 게임 내부 데이터가 필요합니다.  
이 데이터들은 '차트 및 확률' 기능을 통해 뒤끝 콘솔에 업로드하고, 클라이언트(게임)에서 다운로드하여 사용합니다.  
차트와 확률에 대한 더 자세한 설명은 가이드 문서의 [차트 관리](/guide/console-guide/backnd-base/chart)와 [확률 관리](/guide/console-guide/backnd-base/probability)를 확인해 주세요.  

### 1. 차트 파일 확인

프로젝트 폴더 안의 `Assets/Food Truck Venture Datas` 폴더에서 차트 데이터 파일을 확인합니다.

### 2. 차트 폴더 생성

뒤끝 콘솔 > 차트 메뉴에서 아래 표의 7개 차트 폴더를 생성합니다.  
차트에서 폴더 생성을 누르면 폴더 생성 창이 뜨고 내용을 입력해 폴더를 생성합니다.

![image](/img/docs/guide/base/example-games/idle-tycoon/createChartFolder.png)

생성할 차트 폴더는 7개이며 이름과 설명은 아래와 같습니다.  

|폴더명              | 폴더 설명        |
|--------------------|-----------------|
|StationUpgradeList  | List of Upgrade |
|StageUpgradeList    |                 |
|InGameItem          | List of items   |
|ShopData            | Shop Items Datas|
|DailyReward         | Daily Reward    |
|CharacterData       | Character Data  |
|StageDefaultDataList|                 |

### 3.  차트 생성 및 업로드

각 폴더 안에 차트를 생성한 뒤, 차트 파일을 업로드하고 **파일 적용**을 눌러야 합니다.

1. 폴더 안에서 차트 생성을 눌러 차트의 이름과 설명을 작성하고 우편 사용여부를 선택하여 차트를 생성합니다
![image](/img/docs/guide/base/example-games/idle-tycoon/createChart.png)

2. 생성된 차트를 클릭하고 이후 나오는 화면에서 차트 파일 업로드를 눌러 다운 받은 차트 데이터 파일을 업로드 합니다.
![image](/img/docs/guide/base/example-games/idle-tycoon/uploadChart.png)

3. 업로드 한 차트 파일의 체크박스에 체크 하신 뒤 차트 파일 적용 버튼을 클릭하여 차트 파일을 적용 시킵니다.
![image](/img/docs/guide/base/example-games/idle-tycoon/applyChart.png)

:::warning 주의 
차트 데이터 파일 업로드 이후 반드시 차트 파일 적용을 해야합니다.  
파일 적용을 하지 않으면 뒤끝 콘솔에 업로드한 데이터를 클라이언트로 가져올수 없습니다.
:::

아래의 표에 작성된 내용을 참고로 폴더에 차트 데이터를 업로드 해야합니다.  

#### StationUpgradeList 폴더

|차트명           | 업로드할 파일명       | 차트 설명                            |우편 기능 |
|-----------------|---------------------|-------------------------------------|---------|
|STG0001_STS0001  |STG0001_STS0001.xlsx | Burrito Station's List of Upgrade   |사용안함 |
|STG0002_STS0001  |STG0002_STS0001.xlsx | Taco Station's List of Upgrades     |사용안함 |
|STG0002_STS0002  |STG0002_STS0002.xlsx | Juice Station's List of Upgrade     |사용안함 |
|STG0003_STS0001  |STG0003_STS0001.xlsx | Hot Dog Station's List of Upgrades  |사용안함 |
|STG0003_STS0002  |STG0003_STS0002.xlsx | Cola Station's List of Upgrades     |사용안함 |
|STG0003_STS0003  |STG0003_STS0003.xlsx | Burrito Station's List of Upgrade   |사용안함 |
 
#### StageUpgradeList 폴더

|차트명       |업로드할 파일명| 차트 설명              |우편 기능|
|-------------|-------------|------------------------|--------|
|	UE_STG0001  | STG0001.csv | Upgrade Effect Stage 1 |사용안함|
|	UE_STG0002  | STG0002.csv | Upgrade Effect Stage 2 |사용안함|
|	UE_STG0003  |	STG0003.csv | Upgrade Effect Stage 3 |사용안함|

#### InGameItem 폴더

|차트명        |업로드할 파일명         | 차트 설명              |우편 기능|
|--------------|----------------------|------------------------|--------|
|	DressDatas   |DressChartData.xlsx   |List of item reward data|사용     |
|	LootBoxDatas |LootBoxChartData.xlsx |List of loot box data   |사용안함|
|RewardItemData|RewardItemData.xlsx   |List of dress data      |사용안함|

#### ShopData 폴더

|차트명          |업로드할 파일명      | 차트 설명                |우편 기능|
|---------------|--------------------|-------------------------|--------|
|GemsShopData   |GemsShopData.xlsx   |Gems sale data in shop   |사용안함|
|BundleShopData |BundleShopData.xlsx |Bundle sale data in shop |사용안함|
|LootboxShopData|LootboxShopData.xlsx|Lootbox sale data in shop|사용안함|

#### DailyReward 폴더

|차트명          |업로드할 파일명     | 차트 설명     |우편 기능|
|---------------|-------------------|---------------|--------|
|DailyReward    |DailyReward.xlsx   | Daily Reward  |사용 안함|

#### CharacterData 폴더

|차트명          |업로드할 파일명      | 차트 설명               |우편 기능|
|---------------|--------------------|------------------------|-------|
|SpecialCustomer|SpecialCustomer.xlsx|List of special customer|사용안함|

#### StageDefaultDataList 폴더

|차트명          |업로드할 파일명       | 차트 설명             |우편 기능|
|----------------|---------------------|--------------------|-------|
|StageDefaultData|StageDefaultData.xlsx|         ---        |사용안함|


:::tip
우편 기능이 ‘사용’인 경우, 해당 차트 데이터는 게임 내 우편으로 지급될 수 있는 보상 아이템으로 사용됩니다.
:::

### 4. 확률 생성 및 업로드

뒤끝 콘솔 > 확률 메뉴에서 ‘Loot Box Gacha Data’ 확률을 생성하고 파일을 업로드 및 적용합니다.  
아래를 참고하여 확률을 생성합니다.
- 확률 명 : Loot Box Gacha Data  
  - 확률 설명 : Loot Box Probability Data  

![image](/img/docs/guide/base/example-games/idle-tycoon/createProbability.png)

확률 파일 업로드를 눌러 다운 받은 확률 데이터 파일을 업로드 합니다.  
업로드할 파일 이름은 아래와 같습니다.

- LootBoxProbability.xlsx

![image](/img/docs/guide/base/example-games/idle-tycoon/uploadProbability.png)


업로드한 확률 파일을 파일 적용 버튼을 눌러 적용 시킵니다.

![image](/img/docs/guide/base/example-games/idle-tycoon/applyProbability.png)

:::warning 주의 
확률 데이터 파일 업로드 이후 반드시 확률 파일 적용을 해야합니다.  
파일 적용을 하지 않으면 뒤끝 콘솔에 업로드한 데이터를 클라이언트로 가져올수 없습니다.
:::
