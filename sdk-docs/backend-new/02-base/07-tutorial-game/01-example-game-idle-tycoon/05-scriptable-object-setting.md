---
sidebar_label: 유니티 스크립터블 오브젝트 세팅
description: "유니티 스크립터블 오브젝트 세팅"
sidebar_position: 5
---

# 유니티 스크립터블 오브젝트 세팅

## 스크립터블 오브젝트에 차트/확률 파일 ID 설정

차트 및 확률 데이터를 게임 내에서 사용하려면, 해당 데이터의 **파일 ID**를  
Unity 프로젝트 내의 **스크립터블 오브젝트(ScriptableObject)**에 연결해야 합니다.    
뒤끝 콘솔 차트/확률에서 가져온 데이터는 스크립터블 오브젝트에 저장되어 게임에 반영 됩니다.  
파일 ID는 차트/확률 파일을 업로드하면 생성되며 반드시 차트/확률 파일을 적용해야 해당 데이터에 접근 가능합니다.  

### 1. 차트 데이터를 스크립터블 오브젝트에 연결  

- 1. 차트에 적용된 파일 ID를 복사합니다.
![image](/img/docs/guide/base/example-games/idle-tycoon/chartID.png)

- 2. 데이터를 받을 스크립터블 오브젝트를 찾아 선택하고 Inspector창에 위에서 복사한 차트 파일 ID를 붙여 넣습니다. 
![image](/img/docs/guide/base/example-games/idle-tycoon/scriptableObject.png)

**스크립터블 오브젝트의 기본 경로는 Assets/ScriptableObjects 입니다.**  
아래 표의 폴더 구조를 따라 접근하거나 파일명을 Unity에서 직접 검색하셔도 됩니다.  
  
예를 들어 파일명이 [Lemonade]인 스크립터블 오브젝트의 파일 위치로 접근하기 위해서는  
 Assets/ScriptableObjects/Kitchen Level Data/KitchenDataPerStage/STG0001 경로를 따라 접근 하시면 [Lemonade]파일을 찾을 수 있습니다.

#### 차트 StageUpgradeList 폴더 
|차트 데이터     |스크립터블 오브젝트 파일 위치                     |파일명   |
|---------------|------------------------------------------------|--------|
|STG0001_STS0001| Kitchen Level Data/KitchenDataPerStage/STG0001 |Lemonade|
|STG0002_STS0001| Kitchen Level Data/KitchenDataPerStage/STG0002 |Cola    |
|STG0002_STS0002| Kitchen Level Data/KitchenDataPerStage/STG0002 |HotDog  |
|STG0003_STS0001| Kitchen Level Data/KitchenDataPerStage/STG0003 |Juice   |
|STG0003_STS0002| Kitchen Level Data/KitchenDataPerStage/STG0003 |Taco    |
|STG0003_STS0003| Kitchen Level Data/KitchenDataPerStage/STG0003 |Burrito |

#### 차트 StationUpgradeList 폴더 
|차트 데이터     |스크립터블 오브젝트 파일 위치    |파일명  |
|---------------|-------------------------------|-------|
|UE_STG0001     | Stage Data/Stage Upgrade Data |STG0001|
|UE_STG0002     | Stage Data/Stage Upgrade Data |STG0002|
|UE_STG0003     | Stage Data/Stage Upgrade Data |STG0003|

#### 차트 InGameItem 폴더 
|차트 데이터     |SO 파일 위치   |파일명      |
|---------------|---------------|------------|
|DressDatas     | Dress Data    |DressData   |
|LootBoxDatas   | Loot Box Data |LootBoxDatas|

#### 차트 ShopData 폴더 
|차트 데이터     |SO 파일 위치|파일명          |        
|---------------|-----------|---------------|
|GemsShopData   | Shop Data |GemsSaleData   |
|BundleShopData | Shop Data |BundleSaleData |
|LootboxShopData| Shop Data |LootboxSaleData|

#### 차트 DailyReward 폴더 
|차트 데이터     |SO 파일 위치       |파일명                |        
|---------------|-------------------|---------------------|
|	DailyReward   | Daily Reward Data |DailyRewardBatchData |

#### 차트 CharacterData 폴더 
|차트 데이터     |SO 파일 위치   |파일명            |        
|---------------|---------------|-----------------|
|SpecialCustomer| Customer Data |CustomerBatchData|

#### 차트 StageDefaultDataList 폴더 
|차트 데이터      |SO 파일 위치  |파일명                       |        
|----------------|-------------|----------------------------|
|StageDefaultData|Stage Data   |StageDefaultDatasCollections|


### 2. 확률 데이터를 스크립터블 오브잭트에 연결  

확률 데이터 역시 차트와 동일하게 **파일 ID를 복사 후 스크립터블 오브젝트에 입력**해야 게임에서 정상적으로 사용할 수 있습니다.  

- 1. 확률에 적용된 파일 ID를 복사합니다.
![image](/img/docs/guide/base/example-games/idle-tycoon/probabilityID.png)

- 2. 스크립터블 오브젝트를 선택하고 Inspector창에 위에서 복사한 확률 파일 ID를 붙여 넣습니다. 
![image](/img/docs/guide/base/example-games/idle-tycoon/scriptableObjectProb.png)

**스크립터블 오브젝트의 기본 경로는 Assets/ScriptableObjects 입니다.**  
더 자세한 위치와 스크립터블 오브젝트 파일명은 아래의 표를 참고해주세요.
기본 경로에 아래 표의 파일 위치로 접근하거나 파일명으로 검색하시면 빠르게 찾을 수 있습니다.


#### 확률 Loot Box Gacha Data

|확률 데이터        |파일 위치    |파일명                 |        
|------------------|-------------|----------------------|
|LootBoxProbability|Loot Box Data|LootBoxProbabilityData|

### 3. 게임 시작

모든 설정이 완료되었습니다.  
이제 Unity에서 `StartScene` 씬을 열고 ▶ 버튼을 클릭하여 예제 게임을 실행해보세요!  

> 설정된 차트/확률 데이터가 게임에 정상 반영되는지 꼭 확인해보시기 바랍니다.  
