---
sidebar_label: 구글 영수증 검증
---

# Google.ValidateReceipt

public Task< ValidateReceiptResult > **ValidateReceiptAsync**(string **receipt**, string **description**);  
public Task< ValidateReceiptResult > **ValidateReceiptAsync**(string **receipt**, string **description**, decimal **price**, string **currency**);  
public Task< ValidateReceiptResult > **ValidateReceiptAsync**(string **receipt**, string **description** , bool **isSubscription**);  
public Task< ValidateReceiptResult > **ValidateReceiptAsync**(string **receipt**, string **description**, bool **isSubscription**, decimal **price**, string **currency**);  


:::danger 영수증 검증 이용 시 주의 사항
**Unity IAP 5.X 버전 미지원 안내**   
Unity IAP 5.x 버전이 정식 출시 되었으나 내부구조가 기존과 상이하여 뒤끝의 업데이트 지원이 필요합니다.  
**뒤끝의 Unity IAP 5.X 버전 공식 지원 전까지는 영수증 검증 기능 이용 시 Unity IAP 4.13.0을 사용해 주시기 바랍니다.**  

**일부 국가의 iap Price 데이터 치환 사용 안내**   
화폐 숫자 표현에서 `.`(소수점)을 `,`(쉼표)로 사용하는 일부 국가들이 존재합니다.  
이러한 국가에서는 각 스토어에서 제공하는 iapPrice 값도 `,`가 포함된 형태로 전달됩니다.  
뒤끝의 영수증 검증은 숫자 데이터만 처리할 수 있기 때문에, `,`가 포함된 값을 iapPrice 파라미터로 그대로 사용할 경우 금액 정보가 정상적으로 반영되지 않고 0으로 처리됩니다.  
**`,` 가 포함된 iapPrice 데이터는 `.`로 치환하여 사용해 주세요.**  
:::

## 인앱상품 이름 및 설명에 특수문자 포함 시 에러 발생

구글 플레이 콘솔에서 설정하는 `인앱 상품 > 상품 세부 정보`의 이름과 설명에는 일부 특수문자 사용이 불가능합니다.  
인앱을 생성할 때 이름과 설명에 다음과 같은 값이 포함되어있는지 확인해주세요.  

### 사용 불가능한 특수문자

| 특수문자 | 한글명      |
| -------- | ----------- |
| '        | 작은 따옴표 |
| \        | 역슬래시    |

### 사용 가능한 특수문자

| 특수문자 | 한글명   |
| -------- | -------- |
| .        | 마침표   |
| ,        | 쉼표     |
| "        | 큰따옴표 |
| ()      | 괄호     |
| !        | 느낌표   |
| ?        | 물음표   |
| ~        | 물결     |
| @        | 골뱅이   |
| \*       | 곱하기   |
| +        | 더하기   |
| -        | 빼기     |
| /        | 슬래시   |

**표기가 되지 않은 특수문자를 사용할 경우에는 영수증 검증이 정상적으로 진행되는지 확인 후 적용해주세요.**  

## 파라미터

| Value              | Type   | Description                                           | default |
| ------------------ | ------ | ----------------------------------------------------- | :-----: |
| receipt            | string | Purchasing.PurchaseEventArgs.purchasedProduct.receipt |    -    |
| description        | string | 추가로 저장하고자 하는 내용                           |    -    |
| isSubscription     | bool   | 해당 상품의 구독 상품 여부. **true일 경우 구독 상품** |  false  |

## 설명

[유니티에서 지원하는 IAP 서비스](https://docs.unity3d.com/Packages/com.unity.purchasing@4.12/manual/index.html)의 IStoreListener.ProcessPurchase()에서 구매한 상품에 대한 영수증을 받아 뒤끝 서버를 통해 영수증 검증을 받습니다.  

- 뒤끝은 **영수증 자체의 유효성과, 구매한 productId**를 검증합니다.  
- 뒤끝 로그인 없이 뒤끝 영수증 검증 기능을 사용하는 것은 불가능합니다.  

> 구글 영수증 검증을 위해서는 뒤끝 및 구글 콘솔 설정이 필요합니다.  
> 자세한 설명은 [구글 결제 콘솔 설정 문서](/sdk-docs/backend/base/receipt/android/setting-google-console)을 참고해 주세요.  

### 금액 표시(선택 사항)
뒤끝 콘솔의 영수증 검증 항목에서 해당 구매 내역의 금액을 표시하고자 할 경우, 인자값 iapPrice와 iapCurrency를 추가해야합니다.

```js
private async Task GetCheckGoogleReceiptWithPrice(PurchaseEventArgs purchaseEvent)
{
    string receiptToken = purchaseEvent.purchasedProduct.receipt;
    var reqResult = await BackndIAP.Google.ValidateReceiptAsync(
            receipt: receiptToken,
            description: "구매 했습니다",
            isSubscription: false,
            price: purchaseEvent.purchasedProduct.metadata.localizedPrice,
            currency: purchaseEvent.purchasedProduct.metadata.isoCurrencyCode);
}
```

## Example

### Task 방식
```js
public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs args)
{
    _ = ValidateReceipt(args);

    // Return a flag indicating whether this product has completely been received, or if the application needs
    // to be reminded of this purchase at next app launch. Use PurchaseProcessingResult.Pending when still
    // saving purchased products to the cloud, and when that save is delayed.  
    return PurchaseProcessingResult.Complete;
}

private async Task ValidateReceipt(PurchaseEventArgs args)
{
    string receiptJson = args.purchasedProduct.receipt;
    
    // 금액을 콘솔에서 표시하고자 할 경우, price와 currency 추가.
    decimal iapPrice = args.purchasedProduct.metadata.localizedPrice;
    string iapCurrency = args.purchasedProduct.metadata.isoCurrencyCode;
    // 뒤끝 영수증 검증 처리
    var validationResult = await BackndIAP.Google.ValidateReceiptAsync(receiptJson, "receiptDescription", iapPrice, iapCurrency);
    // 영수증 검증에 성공한 경우
    if (validationResult.IsSuccess())
    {
        // 구매 성공한 제품에 대한 id 체크하여 그에 맞는 보상
        // A consumable product has been purchased by this user.  
        if (string.Equals(args.purchasedProduct.definition.id, "consumable-product-id", StringComparison.Ordinal))
        {
            Debug.Log(string.Format("ProcessPurchase: PASS. Product: '{0}'", args.purchasedProduct.definition.id));
            // The consumable item has been successfully purchased, add 100 coins to the player's in-game score.  
            ScoreManager.score += 100;
        }
        // Or ... a non-consumable product has been purchased by this user.  
        else if (string.Equals(args.purchasedProduct.definition.id, "nonconsumable-product-id", StringComparison.Ordinal))
        {
            Debug.Log(string.Format("ProcessPurchase: PASS. Product: '{0}'", args.purchasedProduct.definition.id));
            // TODO: The non-consumable item has been successfully purchased, grant this item to the player.  
        }
        // Or ... a subscription product has been purchased by this user.  
        else if (string.Equals(args.purchasedProduct.definition.id, "subscription-product-id", StringComparison.Ordinal))
        {
            Debug.Log(string.Format("ProcessPurchase: PASS. Product: '{0}'", args.purchasedProduct.definition.id));
            // TODO: The subscription item has been successfully purchased, grant this to the player.  
        }
    }
    // 영수증 검증에 실패한 경우
    else
    {
        // Or ... an unknown product has been purchased by this user. Fill in additional products here....  
        Debug.Log(string.Format("ProcessPurchase: FAIL. Unrecognized product: '{0}'", args.purchasedProduct.definition.id));
    }
}
```

### Callback 방식
```js
public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs args)
{
    string receiptJson = args.purchasedProduct.receipt;
    decimal iapPrice = args.purchasedProduct.metadata.localizedPrice; // 콘솔에서 금액을 표시하고자 할 경우, 입력(iapCurrency와 같이 입력해야합니다.)
    string iapCurrency = args.purchasedProduct.metadata.isoCurrencyCode;

    BackndIAP.Google.ValidateReceipt(args.purchasedProduct.receipt, "receiptDescription", false, iapPrice, iapCurrency, (callback) =>
    {
        // 영수증 검증에 성공한 경우
        if (callback.IsSuccess())
        {
            if (string.Equals(args.purchasedProduct.definition.id, "consumable-product-id", StringComparison.Ordinal))
            {
                Debug.Log(string.Format("ProcessPurchase: PASS. Product: '{0}'", args.purchasedProduct.definition.id));
                // The consumable item has been successfully purchased, add 100 coins to the player's in-game score.  
                ScoreManager.score += 100;
            }
            // Or ... a non-consumable product has been purchased by this user.  
            else if (string.Equals(args.purchasedProduct.definition.id, "nonconsumable-product-id", StringComparison.Ordinal))
            {
                Debug.Log(string.Format("ProcessPurchase: PASS. Product: '{0}'", args.purchasedProduct.definition.id));
                // TODO: The non-consumable item has been successfully purchased, grant this item to the player.  
            }
            // Or ... a subscription product has been purchased by this user.  
            else if (string.Equals(args.purchasedProduct.definition.id, "subscription-product-id", StringComparison.Ordinal))
            {
                Debug.Log(string.Format("ProcessPurchase: PASS. Product: '{0}'", args.purchasedProduct.definition.id));
                // TODO: The subscription item has been successfully purchased, grant this to the player.  
            }
        }
        else
        {
            // 영수증 검증에 실패한 경우
            Debug.Log(string.Format("ProcessPurchase: FAIL. Unrecognized product: '{0}'", args.purchasedProduct.definition.id));
        }
    });

    // Return a flag indicating whether this product has completely been received, or if the application needs
    // to be reminded of this purchase at next app launch. Use PurchaseProcessingResult.Pending when still
    // saving purchased products to the cloud, and when that save is delayed.  
    return PurchaseProcessingResult.Complete;
}
```

## ReturnCase

### Success cases

**검증에 성공한 경우(구독 상품)**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

**검증에 성공한 경우(일반 상품)**  
statusCode : 201  
message : Success  
returnValue : **{"usedDate":"2018-10-15T05:17:49Z"}**  

### Error cases

**유효하지 않은 영수증 토큰**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다

> 1-3번 항목의 JWT 권한의 확인이 필요합니다.  
> 권한을 제대로 설정했는데도 bad token 에러가 발생할 경우 JWT를 재발급하고, 재발급 후에도 에러가 발생할 경우 JWT를 새로 생성한 후 다시 시도해야 합니다.  
> JWT 수정 여부는 바로 적용이 안될 수 있어 1~2시간 후에 다시 시도하는 것을 권장합니다. 

**환불/취소 영수증**  
statusCode : 402  
errorCode : AbnormalReceipt  
message : This receipt has changed status. purchaseState: cancelled

**이미 사용한 영수증 토큰**  
statusCode : 409  
errorCode : UsedReceipt  
message : This receipt has already been used. usedDate: **2018-02-15T04:01:50.000Z**

**이미 사용하거나 취소된 구독 상품의 영수증을 검증한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated receipt, 중복된 receipt 입니다

## ReturnValueJson(구독 상품)

```js
{
    "kind": "androidpublisher#subscriptionPurchaseV2",
    "startTime": "2025-01-09T05:15:19.985Z",
    "regionCode": "국가코드",
    "subscriptionState": "SUBSCRIPTION_STATE_ACTIVE",
    "latestOrderId": "구글주문ID",
    "linkedPurchaseToken": "구글주문토큰",
    "testPurchase": {},
    "acknowledgementState": "ACKNOWLEDGEMENT_STATE_ACKNOWLEDGED",
    "lineItems": [{
        "productId": "상품ID",
        "expiryTime": "2025-01-09T05:18:10.495Z",
        "autoRenewingPlan": {
            "autoRenewEnabled": true,
            "recurringPrice": {
                "currencyCode": "통화코드",
                "units": "금액"
            }
        }
    }]
}
```

---

# Google.ValidateReceipt

public Task< ValidateReceiptResult > **ValidateReceiptAsync**(string **productId**, string **token**, string **description**);  
public Task< ValidateReceiptResult > **ValidateReceiptAsync**(string **productId**, string **token**, string **description**, decimal **price**, string **currency**);  
public Task< ValidateReceiptResult > **ValidateReceiptAsync**(string **productId**, string **token**, string **description** , bool **isSubscription**);  
public Task< ValidateReceiptResult > **ValidateReceiptAsync**(string **productId**, string **token**, string **description**, bool **isSubscription**, decimal **price**, string **currency**);  

## 파라미터

| Value              | Type   | Description                                           | default |
| ------------------ | ------ | ----------------------------------------------------- | :-----: |
| productId          | string | 구매하고자 하는 productId                             |    -    |
| token              | string | 구매 이후에 발행되는 영수증 토큰                      |    -    |
| description        | string | 추가로 저장하고자 하는 내용                           |    -    |
| isSubscription     | bool   | 해당 상품의 구독 상품 여부. **true일 경우 구독 상품** |  false  |

## 설명

유니티에서 제공하는 IAP 서비스를 사용하지 않아도 제품 productID와 영수증 token을 알고 있으면 뒤끝 서버를 통해 영수증을 검증받을 수 있습니다.  

## Example

### Task 방식
```js
var validation = await BackndIAP.Google.ValidateReceiptAsync(productID, receiptToken, "receiptDescription", true);
if (validation.IsSuccess())
{
    // 영수증 검증 성공 시 처리
}
else
{
    // 영수증 검증 실패 시 처리
}
```

### Callback 방식
```js
BackndIAP.Google.ValidateReceipt(productID, receiptToken, "receiptDescription", false, (callback) =>
{
    if (callback.IsSuccess())
    {
        // 영수증 검증 성공 시 처리
    }
    else
    {
        // 영수증 검증 실패 시 처리
    }
});
```

## ReturnCase

### Success cases

**검증에 성공한 경우(구독 상품)**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

**검증에 성공한 경우(일반 상품)**  
statusCode : 201  
message : Success  
returnValue : **{"usedDate":"2018-10-15T05:17:49Z"}**  

### Error cases

**유효하지 않은 영수증 토큰**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다

> JWT 권한의 확인이 필요합니다.  
> 권한을 제대로 설정했는데도 bad token 에러가 발생할 경우 JWT를 재발급하고, 재발급 후에도 에러가 발생할 경우 JWT를 새로 생성한 후 다시 시도해야 합니다.  
> JWT 수정 여부는 바로 적용이 안될 수 있어 1~2시간 후에 다시 시도하는 것을 권장합니다.  

**환불/취소 영수증**  
statusCode : 402  
errorCode : AbnormalReceipt  
message : This receipt has changed status. purchaseState: cancelled

**이미 사용한 영수증 토큰**  
statusCode : 409  
errorCode : UsedReceipt  
message : This receipt has already been used. usedDate: **2018-02-15T04:01:50.000Z**

**이미 사용하거나 취소된 구독 상품의 영수증을 검증한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated receipt, 중복된 receipt 입니다

## ReturnValueJson(구독 상품)

```js
{
    "kind": "androidpublisher#subscriptionPurchaseV2",
    "startTime": "2025-01-09T05:15:19.985Z",
    "regionCode": "국가코드",
    "subscriptionState": "SUBSCRIPTION_STATE_ACTIVE",
    "latestOrderId": "구글주문ID",
    "linkedPurchaseToken": "구글주문토큰",
    "testPurchase": {},
    "acknowledgementState": "ACKNOWLEDGEMENT_STATE_ACKNOWLEDGED",
    "lineItems": [{
        "productId": "상품ID",
        "expiryTime": "2025-01-09T05:18:10.495Z",
        "autoRenewingPlan": {
            "autoRenewEnabled": true,
            "recurringPrice": {
                "currencyCode": "통화코드",
                "units": "금액"
            }
        }
    }]
}
```
