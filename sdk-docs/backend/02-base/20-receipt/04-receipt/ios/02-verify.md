---
sidebar_label: 애플 영수증 검증
---

# IsValidateApplePurchase
public BackendReturnObject **IsValidateApplePurchase**(string **receipt**, string **receiptDescription**);

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

### 사용 불가능한 특수문자
| 특수문자 | 한글명 |
|---|---|
| ' | 작은 따옴표 |
| \ | 역슬래시 |

### 사용 가능한 특수문자
| 특수문자 | 한글명 |
|---|---|
| . | 마침표 |
| , | 쉼표 |
| " | 큰따옴표 |
| () |  괄호 |
| !  | 느낌표 |
| ? | 물음표 |
| ~ | 물결 |
| @ | 골뱅이 |
| *  | 곱하기 |
| + | 더하기 |
| - | 빼기 |
| / | 슬래시 |

**표기가 되지 않은 특수문자를 사용할 경우에는 영수증 검증이 정상적으로 진행되는지 확인 후 적용해주세요.**  

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| receipt | string | Purchasing.PurchaseEventArgs.purchasedProduct.receipt |
| receiptDescription | string | 추가로 저장하고자 하는 내용 |

## 설명
[유니티에서 지원하는 IAP 서비스](https://docs.unity3d.com/Packages/com.unity.purchasing@4.12/manual/index.html)의 IStoreListener.ProcessPurchase()에서 구매한 상품에 대한 영수증을 받아 뒤끝 서버를 통해 영수증 검증을 받을 수 있습니다.  
* 뒤끝은 **영수증 자체의 유효성과, 구매한 productId**를 검증합니다.  
* 뒤끝 로그인 없이 뒤끝 영수증 검증 기능을 사용하는 것은 불가능합니다.  
* 애플 영수증 검증은 일반 상품에 대한 영수증 검증만 지원합니다. 구독 상품에 대한 영수증 검증만 지원하지 않습니다.  

> 애플 영수증 검증을 위해서는 애플 콘솔 설정이 필요합니다. 
자세한 설명은 [애플 결제 콘솔 설정 문서](/sdk-docs/backend/base/receipt/receipt/ios/setting-google-console)을 참고해 주세요.  

### 금액 표시(선택 사항)
뒤끝 콘솔의 영수증 검증 항목에서 해당 구매 내역의 금액을 표시하고자 할 경우, 인자값 iapPrice와 iapCurrency를 추가해야합니다.

```js
 private void GetCheckGoogleReceiptWithPrice(PurchaseEventArgs purchaseEvent)
{
    string receiptToken = purchaseEvent.purchasedProduct.receipt;
    var bro = Backend.Receipt.IsValidateApplePurchase(
            json: receiptToken,
            receiptDescription: "구매 했습니다",
            isSubscription: false,
            iapPrice: purchaseEvent.purchasedProduct.metadata.localizedPrice,
            iapCurrency: purchaseEvent.purchasedProduct.metadata.isoCurrencyCode);
}
```

## Example
```js
// example
// unity iap servcie process 
public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs args) 
{
    string receiptJson = args.purchasedProduct.receipt;

    /*
    뒤끝 영수증 검증 처리
    */
    BackendReturnObject validation = Backend.Receipt.IsValidateApplePurchase(receiptJson, "receiptDescription");

    // 금액을 콘솔에서 표시하고자 할 경우
    decimal iapPrice = args.purchasedProduct.metadata.localizedPrice;
    string iapCurrency = args.purchasedProduct.metadata.isoCurrencyCode;
    BackendReturnObject validation = Backend.Receipt.IsValidateGooglePurchase(receiptJson, "receiptDescription", iapPrice, iapCurrency);

    // 영수증 검증에 성공한 경우
    if(validation.IsSuccess())
    {
        // 구매 성공한 제품에 대한 id 체크하여 그에 맞는 보상 
        // A consumable product has been purchased by this user.  
        if(string.Equals(args.purchasedProduct.definition.id, kProductIDConsumable, StringComparison.Ordinal))
        {
            Debug.Log(string.Format("ProcessPurchase: PASS. Product: '{0}'", args.purchasedProduct.definition.id));
            // The consumable item has been successfully purchased, add 100 coins to the player's in-game score.  
            ScoreManager.score += 100;
        }
        // Or ... a non-consumable product has been purchased by this user.  
        else if(string.Equals(args.purchasedProduct.definition.id, kProductIDNonConsumable, StringComparison.Ordinal))
        {
            Debug.Log(string.Format("ProcessPurchase: PASS. Product: '{0}'", args.purchasedProduct.definition.id));
            // TODO: The non-consumable item has been successfully purchased, grant this item to the player.  
        }
        // Or ... a subscription product has been purchased by this user.  
        else if(string.Equals(args.purchasedProduct.definition.id, kProductIDSubscription, StringComparison.Ordinal))
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

    // Return a flag indicating whether this product has completely been received, or if the application needs 
    // to be reminded of this purchase at next app launch. Use PurchaseProcessingResult.Pending when still 
    // saving purchased products to the cloud, and when that save is delayed. 
    return PurchaseProcessingResult.Complete;
}
```

## ReturnCase

### Success cases

**성공한 경우**  
statusCode : 201  
message : Success  
returnValue : **{"usedDate":"2018-10-15T05:17:49Z"}**  

### Error cases

**유효하지 않은 영수증 토큰**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다

**위변조된 영수증 토큰**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다

**유효하지 않은 영수증 토큰 / 위변조된 영수증 토큰**

> 인증 설정은 제대로 되었으나, Apple 측 토큰 조회 결과 구매한 이력이 없는 경우

```json
{
    "statusCode": 400,
    "errorCode": "BadParameterException",
    "message": "bad token, 잘못된 token 입니다",
    "errorData": {
        "errorCode": 404,
        "apiError": 4040010,
        "errorType": "TRANSACTION_NOT_FOUND",
        "message": "Transaction id not found."
    }
}
```

**뒤끝 콘솔 Apple 인증정보에 문제가 발생한 경우**

```json
{
    "statusCode": 400,
    "errorCode": "BadParameterException",
    "message": "bad token, 잘못된 token 입니다",
    "errorData": {
        "errorCode": 401,
        "apiError": null,
        "errorType": "UNAUTHORIZED",
        "message": "Apple API authentication failed. Check server configuration."
    }
}
```

**환불/취소 영수증**  
statusCode : 402  
errorCode : AbnormalReceipt  
message : This receipt has changed status. purchaseState: cancelled

**이미 사용한 영수증 토큰**  
statusCode : 409  
errorCode : UsedReceipt  
message : This receipt has already been used. usedDate: **2018-02-15T04:01:50.000Z**

```js
{
    "statusCode": 409,
    "errorCode": "UsedReceipt",
    "message": "This receipt has already been used. usedDate: 2026-01-23T06:06:18.000Z",
    "errorData": {
        "errorCode": 409,
        "errorType": "CONFLICT",
        "receiptInfo": {
            "inDate": "2026-01-23T06:06:18.000Z",
            "details": "{\"itemId\":\"10001\",\"itemName\":\"골드 100개\"}"
        }
    }
}
```
