---
sidebar_label: "인앱 결제, 영수증 검증 설정"
sidebar_position: "6"
description: "인앱 결제 및 영수증 검증"
---

# 인앱 결제 및 영수증 검증

:::info 인앱 결제는 유니티 에디터에서만 동작 하도록 기본 세팅 되었습니다.  
예제 프로젝트에는 인앱결제 로직이 구현되어 있지만, Android / iOS 환경에서 실제 뒤끝 계정으로 인앱결제가 이루어지는 것을 방지하기 위해 유니티 에디터에서만 동작 하도록 설정되었습니다.  
Android / iOS 환경에서 결제 테스트를 진행하기 위해서는 개인 개발자 계정에 앱을 등록 하신 뒤 인앱결제 설정을 완료하여 테스트를 진행하시기 바랍니다.  
:::

인앱결제와 영수증 검증을 하기위한 방법은 [구글 플레이 스토어 설정](/guide/console-guide/server-setting/store/googlestore) 또는 [앱 스토어 설정](/guide/console-guide/server-setting/store/appstore)를 참고 하시기 바랍니다.  

프로젝트에서 해당 파일을 열어 코드를 확인하시면 인앱결제 및 영수증 검증이 구현되어 있으니 참고하시길 바랍니다.
```js
 - CodeIAPManager.cs
 - ShopManager.cs
 - BackndPurchasing.cs
   ProcessPurchase() //영수증 검증 로직
```
기본적으로 Android / iOS 환경에서 사용자의 실수로 인앱결제가 발생하는 상황을 막기 위해 상점UI가 나타나지 않게 세팅 되었습니다.  
Android / iOS 환경에서 정상적인 인앱결제 테스트를 진행 하기 위해서는 프로젝트 내에 ShopManager.cs파일을 열어 아래와 같이 SetActiveUI함수를 수정해야 합니다.  
유니티 에디터에서는 수정하지 않아도 정상 작동하여 테스트가 가능 합니다.
```js
        private void SetActiveUI(bool state)
        {
//조건부 컴파일 지시문 주석처리
//#if UNITY_EDITOR  
            _ui.SetActive(state);
//#endif
        }
```

iOS 환경에서 추가로 처리해야하는 부분은 아래와 같습니다.  

```js
        // iOS의 경우  
        private void Awake()
        {
        ...  
#if UNITY_IOS  
            //상점 버튼 비활성화 주석 처리
            //_openShopButton.gameObject.SetActive(false);
#endif
        }
```

위와 같이 처리하면 Android / iOS 환경에서도 UI가 정상 표시됩니다.

## 구글 플레이 콘솔 설정

안드로이드 환경에서 인앱결제를 테스트 하기 위해서는 구글 플레이 콘솔에 앱을 등록하신뒤 인앱 상품을 세팅해야 합니다.  
테스트를 위해 세팅 해야하는 제품 ID와 상품이름은 아래의 표를 참고하시기 바랍니다.  
차트 세팅을 위해 다운 받으신 엑셀 데이터에서도 확인 가능합니다.  

![image](/img/docs/guide/base/example-games/idle-tycoon/inAppGoods.png)  

## 앱스토어 커넥트 설정

iOS 환경에서는 앱스토어 커넥트에 샘플게임을 등록 하신 뒤에 들어가서 앱 내 구입 항목에서 인앱 상품을 세팅하시면 됩니다.  
세팅해야하는 제품 ID와 상품이름 아래의 표를 참고 하시기 바랍니다.

![image](/img/docs/guide/base/example-games/idle-tycoon/inAppGoodsApple.png)  


|제품 ID       | 상품 이름          |
|--------------|-------------------|
|xxgems_01     |Single Gem         |
|xxgems_02     |Handful of Gems    |
|xxgems_03     |Heap of Gems       |
|xxgems_04     |Small box of Gems  |
|xxgems_05     |Medium box of Gems |
|xxgems_06     |Trunks of Gems     |
|xxbdl_stlar   |Starter Pack Large |
|xxbdl_stmed   |Starter Pack Medium|
|xxbdl_stmini  |Starter Pack Mini  |
|xxbdl_fashmed |Fashion Pack Medium|
|xxbdl_fashmini|Fashion Pack Mini  |

엑셀 데이터 파일
- GemsShopData.xlsx
- BundleShopData.xlsx

# 구매 복원

**CodeIAPManager.cs 파일**을 열어보면 **OnInitialized함수** 부분에 구현 되어 있습니다.  
아래는 구매 복원이 구현된 코드의 부분이며 더 자세한 내용은 파일을 확인 하세요.  

```js
        if (product.hasReceipt) // purchase restoration
        {
            if (iapItem.InAppProductType == ProductType.NonConsumable)
                iapItem.InAppPurchaseIsOwned = true;

        else if (iapItem.InAppProductType == ProductType.Subscription)
                iapItem.InAppPurchaseIsOwned =
                new SubscriptionManager(product, null).getSubscriptionInfo().isSubscribed() == Result.True;
        }
```
