---
sidebar_label: Unity Purchasing 연동 예제
---

# Unity Purchasing(IAP 5.x) 연동 예제

Unity Purchasing(IAP 5.x) 환경에서 뒤끝 영수증 검증을 연동하는 전체 예제입니다.

:::info 사전 준비
- Unity Purchasing 패키지(5.0 이상)가 프로젝트에 설치되어 있어야 합니다.
- 플랫폼별 콘솔 설정이 완료되어 있어야 합니다.
  - 구글: [구글 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/android/setting-google-console)
  - 애플: [애플 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/ios/setting-google-console)
  - 원스토어: [원스토어 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/onestore/setting-onestore-console)
:::

:::danger Unity IAP 4.x에서 마이그레이션 시 주의 사항
Unity IAP 5.x에서는 구매 흐름이 크게 변경되었습니다.
- `IStoreListener`/`IDetailedStoreListener` 인터페이스가 `StoreController` 이벤트 방식으로 대체되었습니다.
- `ProcessPurchase` 콜백 대신 `OnPurchasePending` 이벤트에서 구매를 처리하고, `ConfirmPurchase`를 호출하여 구매를 확정합니다.
- `Product.receipt` 속성이 deprecated 되었습니다. `PendingOrder.Info.Receipt` 또는 iOS의 경우 `PendingOrder.Info.Apple.jwsRepresentation`을 사용해야 합니다.
:::

## 전체 연동 흐름

```
1. 스토어 연결 (Connect)
2. 상품 조회 (FetchProducts)
3. 구매 요청 (PurchaseProduct)
4. 구매 대기 (PendingOrder)
5. 뒤끝 영수증 검증 (ValidateReceipt)
6. 구매 확정 (ConfirmPurchase)
```

## IStoreBackend 인터페이스

스토어 연동을 추상화하는 인터페이스를 정의합니다.

```js
public interface IStoreBackend
{
    bool IsInitialized { get; }
    void Initialize(IStoreBackendCallbacks callbacks);
    void PurchaseProduct(string shopItemId);
    string GetLocalizedPrice(string shopItemId);
    void RestoreTransactions(Action<bool, string> callback);
    void Shutdown();
}

public interface IStoreBackendCallbacks
{
    void OnStoreInitialized(Dictionary<string, string> localizedPrices);
    void OnStoreInitializeFailed(string error);
    void OnStorePurchaseSuccess(string shopItemId);
    void OnStorePurchaseFailed(string shopItemId, string error);
}
```

## 초기화

`StoreController`에 이벤트 핸들러를 등록하고, 스토어에 연결한 뒤 상품을 조회합니다.

```js
private StoreController _storeController;

private async Task InitializeAsync()
{
    _storeController = UnityIAPServices.StoreController();

    _storeController.OnProductsFetched += HandleProductsFetched;
    _storeController.OnProductsFetchFailed += HandleProductsFetchFailed;
    _storeController.OnPurchasePending += HandlePurchasePending;
    _storeController.OnPurchaseConfirmed += HandlePurchaseConfirmed;
    _storeController.OnPurchaseFailed += HandlePurchaseFailed;
    _storeController.OnPurchasesFetched += HandlePurchasesFetched;
    _storeController.OnPurchasesFetchFailed += HandlePurchasesFetchFailed;
    _storeController.OnStoreDisconnected += HandleStoreDisconnected;

    await _storeController.Connect();

    var productDefinitions = ShopCatalog.GetUniqueProductDefinitions();
    _storeController.FetchProducts(productDefinitions);
}
```

## 상품 조회 완료 처리

상품 조회가 완료되면 현지화된 가격 정보를 캐싱하고, 미처리된 구매 내역을 확인합니다.

```js
private readonly Dictionary<string, string> _localizedPrices = new Dictionary<string, string>();

private void HandleProductsFetched(List<Product> products)
{
    _localizedPrices.Clear();
    foreach (var product in products)
    {
        if (product.metadata != null)
        {
            _localizedPrices[product.definition.id] = product.metadata.localizedPriceString;
        }
    }

    IsInitialized = true;
    _callbacks.OnStoreInitialized(new Dictionary<string, string>(_localizedPrices));

    // 미완료된 구매 내역 확인
    _storeController.FetchPurchases();
}
```

## 구매 요청

```js
public void PurchaseProduct(string shopItemId)
{
    var shopItem = ShopCatalog.GetItemById(shopItemId);
    if (shopItem == null)
    {
        _callbacks.OnStorePurchaseFailed(shopItemId, "상품을 찾을 수 없습니다.");
        return;
    }

    _pendingShopItemId = shopItemId;
    _storeController.PurchaseProduct(shopItem.EffectiveProductId);
}
```

## 영수증 검증 (핵심)

구매가 대기 상태(`PendingOrder`)가 되면 뒤끝 영수증 검증을 수행합니다.

### 영수증 추출

Unity IAP 5.x에서는 `Product.receipt`(deprecated) 대신 `PendingOrder.Info`에서 영수증을 추출합니다.
iOS에서는 StoreKit 2 기반의 JWS(JSON Web Signature) 형식 영수증을 사용해야 합니다.

```js
var receipt = pendingOrder.Info.Receipt;

// iOS에서는 StoreKit 2 기반의 JWS 영수증을 사용
if (Application.platform == RuntimePlatform.IPhonePlayer || Application.platform == RuntimePlatform.OSXPlayer)
{
    receipt = pendingOrder.Info.Apple.jwsRepresentation;
}
```

### 영수증 검증 요청

```js
private void ValidateWithReceipt(PendingOrder pendingOrder, string receipt, ShopItem shopItem, string shopItemId)
{
    var param = new ReceiptParam(receipt)
        .AddDetailItemId(shopItem?.Id)            // 게임 내 아이템 ID (스토어 상품 ID가 아님)
        .AddDetailItemName(shopItem?.ItemName)    // 게임 내 아이템 이름
        .AddDetail("iconAddress", shopItem?.IconAddress);

    // 구독 상품인 경우
    if (shopItem?.EffectiveProductType == ShopProductType.Subscription)
        param.Subscription();

    // 비로그인 검증인 경우
    var gamerId = PlayerPrefs.GetString("GamerId", "");
    if (!string.IsNullOrEmpty(gamerId))
        param.SetGamerId(gamerId);

    Backend.Receipt.ValidateReceipt(param, bro =>
    {
        var errorCode = bro.GetErrorCode();

        if (bro.IsSuccess())
        {
            // 최초 검증 성공 → 상품 지급 + 구매 확정
            _storeController.ConfirmPurchase(pendingOrder);
        }
        else if (errorCode == "UsedReceipt")
        {
            // 이미 검증된 영수증 → details의 아이템 정보로 미지급 상품 재지급 가능
            var returnValue = bro.GetReturnValuetoJSON();
            var receiptDetails = returnValue["details"]["receiptInfo"]["details"];
            Debug.Log($"[UnityPurchasingBackend] UsedReceipt details: {receiptDetails}");

            // TODO: receiptDetails에서 게임 아이템 정보를 확인하여 상품 재지급
            _storeController.ConfirmPurchase(pendingOrder);
        }
        else
        {
            // 검증 실패
            _callbacks.OnStorePurchaseFailed(shopItemId,
                $"영수증 검증에 실패했습니다. ({errorCode})");
        }
    });
}
```

:::danger 중요: 구매 확정(ConfirmPurchase)
Unity IAP 5.x에서는 `ConfirmPurchase`를 호출해야 구매가 완료됩니다.
영수증 검증에 성공한 후 반드시 `_storeController.ConfirmPurchase(pendingOrder)`를 호출해 주세요.
`ConfirmPurchase`를 호출하지 않으면 구매가 보류 상태로 남아 자동 환불될 수 있습니다.
:::

## 미완료 구매 복원

앱 재시작 시 미완료된 구매 내역을 복원 처리합니다.
`FetchPurchases`의 결과로 `PendingOrder`가 반환되면, 해당 건에 대해 영수증 검증을 다시 시도합니다.

```js
private void HandlePurchasesFetched(Orders orders)
{
    foreach (var pendingOrder in orders.PendingOrders)
    {
        HandlePurchasePending(pendingOrder);
    }
}
```

:::info UsedReceipt를 활용한 상품 재지급
미완료 구매 복원 시 이미 검증된 영수증에 대해 `UsedReceipt`(409)가 반환될 수 있습니다.
이는 **영수증 검증은 완료되었지만 상품 지급 전에 앱이 종료된 경우**에 해당합니다.

`UsedReceipt` 응답의 `details.receiptInfo.details`에는 최초 검증 시 `AddDetail`로 저장한 게임 아이템 정보(itemId, itemName 등)가 포함되어 있으므로, 이 정보를 활용하여 **미지급된 상품을 재지급**할 수 있습니다.
:::

## 애플 구매 복원 (Restore Transactions)

Apple App Store 심사 가이드라인에 따라, 비소모품(Non-Consumable) 또는 구독(Subscription) 상품을 판매하는 앱은 반드시 **구매 복원(Restore Purchases)** 기능을 제공해야 합니다.
사용자가 앱을 재설치하거나 다른 기기에서 로그인했을 때 이전에 구매한 비소모품/구독 상품을 복원할 수 있어야 합니다.

:::danger Apple 심사 필수 사항
비소모품 또는 자동 갱신 구독 상품을 판매하는 경우, 앱 내에 **"구매 복원" 버튼**이 반드시 있어야 합니다.
이 버튼이 없으면 Apple 앱 심사에서 리젝될 수 있습니다.
:::

```js
public void RestoreTransactions(Action<bool, string> callback)
{
    if (!IsInitialized)
    {
        callback?.Invoke(false, "스토어가 초기화되지 않았습니다.");
        return;
    }

    _storeController.RestoreTransactions((success, error) =>
    {
        Debug.Log($"[UnityPurchasingBackend] Restore result: {success}, error: {error}");
        callback?.Invoke(success, error);
    });
}
```

`RestoreTransactions`가 성공하면, 복원된 구매 내역이 `OnPurchasePending` 이벤트를 통해 전달됩니다.
이때 앞서 구현한 `HandlePurchasePending`에서 영수증 검증 → `ConfirmPurchase` 흐름이 동일하게 실행됩니다.

복원된 구매의 영수증을 검증하면 `UsedReceipt`가 반환됩니다. 이 응답의 `details.receiptInfo.details`에 포함된 게임 아이템 정보를 통해 미지급 상품을 재지급할 수 있습니다.

:::info 구매 복원과 FetchPurchases의 차이
- **FetchPurchases**: 앱 초기화 시 자동으로 호출하여 미완료 구매 내역을 조회합니다. <br/> 소모품(Consumable)의 미확정 구매도 포함됩니다.
- **RestoreTransactions**: 사용자가 명시적으로 "구매 복원" 버튼을 눌렀을 때 호출합니다. <br/> 비소모품/구독 상품의 이전 구매를 스토어에서 다시 가져옵니다. <br/> 복원 시 `UsedReceipt` 응답의 details를 활용하여 상품 재지급이 가능합니다.
:::

### UI 구현 예시

```js
// 구매 복원 버튼의 OnClick 이벤트에 연결
public void OnRestorePurchasesClicked()
{
    // iOS에서만 구매 복원 버튼 동작
    if (Application.platform != RuntimePlatform.IPhonePlayer &&
        Application.platform != RuntimePlatform.OSXPlayer)
    {
        return;
    }

    storeBackend.RestoreTransactions((success, error) =>
    {
        if (success)
        {
            Debug.Log("구매 복원 완료");
            // 복원된 구매는 OnPurchasePending → 영수증 검증 → ConfirmPurchase 흐름으로 자동 처리됩니다.
        }
        else
        {
            Debug.LogError($"구매 복원 실패: {error}");
        }
    });
}
```

## 구매 확정/실패 처리

```js
private void HandlePurchaseConfirmed(Order order)
{
    var purchasedProducts = order.Info?.PurchasedProductInfo;
    if (purchasedProducts != null && purchasedProducts.Count > 0)
    {
        var shopItem = ShopCatalog.GetItemByProductId(purchasedProducts[0].productId);
        if (shopItem != null)
        {
            if (order is ConfirmedOrder)
            {
                _callbacks.OnStorePurchaseSuccess(shopItem.Id);
            }
            else if (order is FailedOrder failedOrder)
            {
                _callbacks.OnStorePurchaseFailed(shopItem.Id, failedOrder.FailureReason.ToString());
            }
        }
    }
}
```

## 리소스 정리

스토어 사용이 끝나면 이벤트 핸들러를 해제합니다.

```js
public void Shutdown()
{
    if (_storeController != null)
    {
        _storeController.OnProductsFetched -= HandleProductsFetched;
        _storeController.OnProductsFetchFailed -= HandleProductsFetchFailed;
        _storeController.OnPurchasePending -= HandlePurchasePending;
        _storeController.OnPurchaseConfirmed -= HandlePurchaseConfirmed;
        _storeController.OnPurchaseFailed -= HandlePurchaseFailed;
        _storeController.OnPurchasesFetched -= HandlePurchasesFetched;
        _storeController.OnPurchasesFetchFailed -= HandlePurchasesFetchFailed;
        _storeController.OnStoreDisconnected -= HandleStoreDisconnected;
    }
}
```

## StoreController 이벤트 요약

| 이벤트 | 타입 | 설명 |
| ------ | ---- | ---- |
| OnProductsFetched | Action<List<Product\>\> | 상품 조회 성공 |
| OnProductsFetchFailed | Action<ProductFetchFailed\> | 상품 조회 실패 |
| OnPurchasePending | Action<PendingOrder\> | 구매 대기 <br/> 영수증 검증 및 ConfirmPurchase 필요 |
| OnPurchaseConfirmed | Action<Order\> | 구매 확정 완료 <br/> ConfirmedOrder 또는 FailedOrder |
| OnPurchaseFailed | Action<FailedOrder\> | 구매 실패 |
| OnPurchaseDeferred | Action<DeferredOrder\> | 구매 지연 <br/> 부모 승인 대기 등 |
| OnPurchasesFetched | Action<Orders\> | 기존 구매 내역 조회 완료 |
| OnPurchasesFetchFailed | Action<PurchasesFetchFailureDescription\> | 기존 구매 내역 조회 실패 |
| OnStoreDisconnected | Action<StoreConnectionFailureDescription\> | 스토어 연결 끊김 |
