---
sidebar_label: 영수증 검증 (Unity IAP 5.x)
description: "뒤끝펑션에서 ValidateReceipt로 인앱 결제 영수증 검증하기"
---

# 영수증 검증

```csharp
public BackendReturnObject ValidateReceipt(ReceiptParam receiptParam);
```

Unity IAP 5.x(Unity Purchasing)로 결제한 영수증이 유효한지 뒤끝 서버에서 확인하는 함수입니다.  
뒤끝은 영수증 자체의 유효성과 구매한 상품 ID를 검증하고, 한 번 검증한 영수증을 다시 검증하면 `UsedReceipt`를 반환합니다.  
Unity IAP 4.x를 사용한다면 [[Legacy] 애플 영수증 검증](/sdk-docs/function/backnd-features/receipt-check/apple)과 [[Legacy] 구글 영수증 검증](/sdk-docs/function/backnd-features/receipt-check/google) 문서를 참고해 주세요.  

스토어별 콘솔 설정과 인앱 상품 이름에 쓸 수 없는 특수문자는 뒤끝베이스의 [ValidateReceipt](/sdk-docs/backend/base/receipt/validate-receipt) 문서를 참고해 주세요.  

:::caution 뒤끝펑션에서는 스토어 타입을 직접 지정합니다
뒤끝펑션은 함수를 호출한 기기의 플랫폼을 알 수 없습니다. 그래서 스토어 타입을 자동으로 정하는 `new ReceiptParam(receipt)`는 함수 안에서 `400 BadRequestException`(`cannot determine store type from platform: None`)을 반환합니다.  
클라이언트가 영수증과 함께 스토어 타입을 보내고, 함수에서는 `new ReceiptParam(스토어 타입, 영수증)`으로 지정해 주세요.  
:::

## 클라이언트에서 보낼 값

| 스토어 | 스토어 타입 | 영수증 | 함께 보낼 값 |
| --- | --- | --- | --- |
| Google Play | `google` | `PendingOrder.Info.Receipt` | 없음 |
| App Store | `apple` | `PendingOrder.Info.Apple.jwsRepresentation` (StoreKit 2의 JWS 영수증) | 없음 |
| ONE Store | `onestore` | `PurchaseData.PurchaseToken` (원스토어 IAP SDK의 결제 토큰) | 결제 금액 `ProductDetail.price`, 통화 `ProductDetail.priceCurrencyCode` |

Google Play와 App Store는 Unity IAP 5.x의 `OnPurchasePending`으로 받은 `PendingOrder`에서 영수증을 꺼내 함수로 보냅니다.  
iOS에서는 `PendingOrder.Info.Receipt` 대신 `PendingOrder.Info.Apple.jwsRepresentation`을 보내야 합니다.  

```csharp
Param param = new Param();

if (Application.platform == RuntimePlatform.IPhonePlayer || Application.platform == RuntimePlatform.OSXPlayer)
{
    // iOS는 StoreKit 2의 JWS 영수증을 보냅니다.
    param.Add("store", "apple");
    param.Add("receipt", pendingOrder.Info.Apple.jwsRepresentation);
}
else
{
    // Android는 Unity IAP가 준 영수증을 그대로 보냅니다.
    param.Add("store", "google");
    param.Add("receipt", pendingOrder.Info.Receipt);
}
```

원스토어는 원스토어 IAP SDK의 `IPurchaseCallback.OnPurchaseSucceeded`로 받은 `PurchaseData`의 결제 토큰을 보냅니다.  
서버가 원스토어 결제 금액을 조회할 수 없으므로, `QueryProductDetails`로 받은 `ProductDetail`의 금액과 통화도 함께 보냅니다.  

```csharp
// purchase는 OnPurchaseSucceeded로 받은 PurchaseData, detail은 같은 상품의 ProductDetail입니다.
Param param = new Param();
param.Add("store", "onestore");
param.Add("receipt", purchase.PurchaseToken);
param.Add("price", detail.price);
param.Add("currency", detail.priceCurrencyCode);
```

함수 호출부터 구매 확정까지의 전체 흐름은 [영수증 검증 및 아이템 지급](/sdk-docs/function/tutorial/receipt-check) 튜토리얼을 참고해 주세요.  

## ReceiptParam

### 생성자

| 생성자 | 설명 |
| --- | --- |
| ReceiptParam(string **receiptType**, string **receipt**) | 스토어 타입(`google`, `apple`, `onestore`)과 영수증을 지정합니다. |

### 메서드

| 메서드 | 설명 |
| --- | --- |
| Subscription() | 구독 상품으로 설정합니다. |
| AddDetailItemId(string **itemId**) | 지급할 게임 아이템 ID를 기록합니다. 같은 스토어 상품으로 여러 게임 아이템을 판매할 때 구분하는 데 사용합니다. |
| AddDetailItemName(string **itemName**) | 지급할 게임 아이템 이름을 기록합니다. |
| AddDetail(string **key**, string **value**) | 상세 정보에 원하는 키와 값을 추가합니다. |
| SetProductId(string **productId**) | 스토어 상품 ID를 지정합니다. ONE Store 등 Google Play와 App Store 외의 스토어에서 필요합니다. Google Play와 App Store는 영수증에 상품 ID가 들어 있으므로 지정하지 않아도 됩니다. |
| SetIsGlobal(bool **isGlobal**) | ONE Store Global 여부를 지정합니다. ONE Store가 아니면 사용하지 않습니다. |
| SetIapPrice(string **iapPrice**, string **iapCurrency**) | 결제 금액과 통화를 지정합니다. ONE Store 등 Google Play와 App Store 외의 스토어에서는 필수입니다. Google Play와 App Store는 서버가 금액을 조회하므로 이 값을 보내지 않습니다. |

- `AddDetail`, `AddDetailItemId`, `AddDetailItemName`으로 기록한 값은 뒤끝 콘솔의 영수증 검증 내역에 함께 저장됩니다. 같은 영수증을 다시 검증하면 `UsedReceipt` 응답에도 들어 있습니다.  
- 금액에 소수점 대신 `,`(쉼표)를 쓰는 국가에서는 스토어가 주는 금액에도 `,`가 들어 있습니다. `,`가 들어 있으면 금액이 0으로 처리되므로 `.`로 바꿔서 `SetIapPrice`에 넣어 주세요.  
- 함수는 호출한 사용자의 로그인 정보로 실행됩니다. 로그인 없이 검증하는 `SetGamerId`는 사용하지 않습니다.  

## Example

### Google Play와 App Store

```csharp
// 클라이언트가 보낸 스토어 타입과 영수증을 읽습니다.
string store = Backend.Content["store"].ToString();
string receipt = Backend.Content["receipt"].ToString();

ReceiptParam receiptParam = new ReceiptParam(store, receipt)
    .AddDetailItemId("10001")
    .AddDetailItemName("골드 100개");

BackendReturnObject bro = Backend.Receipt.ValidateReceipt(receiptParam);

if (bro.IsSuccess())
{
    // 검증에 성공하면 아이템을 지급합니다.
}
```

클라이언트가 보낸 스토어 타입은 그대로 믿지 말고, 함수에서 받을 값(`google`, `apple`, `onestore`)인지 확인한 뒤 사용해 주세요.  

### 구독 상품

```csharp
ReceiptParam receiptParam = new ReceiptParam(store, receipt)
    .Subscription()
    .AddDetailItemId("20001");

BackendReturnObject bro = Backend.Receipt.ValidateReceipt(receiptParam);
```

### ONE Store

```csharp
// 원스토어는 결제 토큰과 함께 결제 금액과 통화를 받습니다.
string purchaseToken = Backend.Content["receipt"].ToString();

// 소수점을 ,(쉼표)로 쓰는 국가의 금액은 .으로 바꿔야 0으로 처리되지 않습니다.
string iapPrice = Backend.Content["price"].ToString().Replace(",", ".");
string iapCurrency = Backend.Content["currency"].ToString();

// 상품 ID는 이 함수가 지급할 아이템의 원스토어 상품 ID로 지정합니다.
ReceiptParam receiptParam = new ReceiptParam("onestore", purchaseToken)
    .SetProductId("gold_100")
    .SetIsGlobal(false)
    .SetIapPrice(iapPrice, iapCurrency)
    .AddDetailItemId("10001");

BackendReturnObject bro = Backend.Receipt.ValidateReceipt(receiptParam);
```

- 원스토어는 `SetProductId`로 지정한 상품 ID와 결제 토큰의 상품이 다르면 검증에 실패합니다(`NoSuchData`). 함수에서 지급할 아이템이 정해져 있다면 상품 ID를 클라이언트가 보낸 값 대신 함수에 고정해, 다른 상품의 결제 토큰으로 아이템을 받지 못하게 해 주세요.  
- 원스토어 글로벌로 출시했다면 `SetIsGlobal(true)`를 사용합니다.  
- 금액과 통화는 클라이언트가 보낸 값이며, 뒤끝 콘솔의 영수증 검증 내역에 표시하는 데 사용됩니다.  

## ReturnCase

### Success cases

| statusCode | message | returnValue |
| --- | --- | --- |
| 200 | Success | `{"usedDate":"2018-10-15T05:17:49Z"}` |

### Error cases

| statusCode | errorCode | 설명 |
| --- | --- | --- |
| 400 | BadRequestException | 함수 안에서 스토어 타입 없이 `new ReceiptParam(receipt)`를 사용한 경우 `cannot determine store type from platform: None` |
| 400 | UndefinedParameterException | 콘솔에 원스토어 정보가 올바르지 않은 경우 `undefined onestore client_id, onestore client_id을(를) 확인할 수 없습니다` |
| 400 | BadParameterException | 영수증이 비어 있는 경우 `undefined token, token을(를) 확인할 수 없습니다` |
| 400 | BadParameterException | Google Play와 App Store 외의 스토어에서 상품 ID가 비어 있는 경우 `undefined productId, productId을(를) 확인할 수 없습니다` |
| 400 | BadParameterException | 영수증이 유효하지 않은 경우 `bad token, 잘못된 token 입니다`. 원인은 `errorData`에서 확인합니다. |
| 402 | AbnormalReceipt | 환불되거나 취소된 영수증 `This receipt has changed status. purchaseState: cancelled` |
| 409 | UsedReceipt | 이미 검증한 영수증 `This receipt has already been used. usedDate: 2018-02-15T04:01:50.000Z` |

### 유효하지 않은 영수증의 원인

`bad token` 오류에는 원인을 담은 `errorData`가 함께 반환됩니다.  

```json
{
    "statusCode": 400,
    "errorCode": "BadParameterException",
    "message": "bad token, 잘못된 token 입니다",
    "errorData": {
        "errorCode": 400,
        "apiError": null,
        "errorType": "INVALID_TOKEN",
        "message": "Invalid Value"
    }
}
```

| 스토어 | errorData.errorType | 원인 |
| --- | --- | --- |
| Google Play | `INVALID_TOKEN` | 유효하지 않은 영수증입니다. |
| Google Play | `PERMISSION_DENIED` | 구글 JWT에 권한이 없습니다. 구글 콘솔의 권한 부여가 진행 중이거나 잘못 설정된 경우가 많습니다. |
| App Store | `TRANSACTION_NOT_FOUND` | 애플에 구매 이력이 없습니다. 유효하지 않거나 위변조된 영수증입니다. |
| App Store | `UNAUTHORIZED` | 뒤끝 콘솔의 애플 인증 정보에 문제가 있습니다. |
| ONE Store | `NoSuchData` | 유효하지 않거나 위변조된 영수증입니다. 상품 ID가 올바르지 않을 때도 반환됩니다. |
| ONE Store | `OauthBadRequest` | 원스토어 clientId 또는 clientSecret이 올바르지 않습니다. |

`PERMISSION_DENIED`가 반환되면 JWT 권한을 확인해 주세요.  
권한을 제대로 설정했는데도 같은 오류가 나면 JWT를 재발급하고, 재발급한 뒤에도 오류가 나면 JWT를 새로 생성한 뒤 다시 시도해 주세요. JWT를 수정한 내용은 바로 적용되지 않을 수 있으므로 1~2시간 후에 다시 시도하는 것을 권장합니다.  

### UsedReceipt 응답 상세

`UsedReceipt`에는 처음 검증한 시각과, 그때 `AddDetail`로 기록한 값이 함께 반환됩니다.  
예를 들어 `AddDetailItemId("10001")`, `AddDetailItemName("골드 100개")`로 검증한 영수증을 다시 검증하면 아래 응답을 받습니다.  

```json
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

| 필드 | 설명 |
| --- | --- |
| errorData.receiptInfo.inDate | 처음 검증한 시각 |
| errorData.receiptInfo.details | 처음 검증할 때 기록한 상세 정보(JSON 문자열) |

함수에서는 아래와 같이 읽습니다.  

```csharp
if (bro.GetErrorCode() == "UsedReceipt")
{
    // 처음 검증할 때 AddDetail로 기록한 값이 JSON 문자열로 들어 있습니다.
    string details = bro.GetReturnValuetoJSON()["errorData"]["receiptInfo"]["details"].ToString();
}
```

검증은 끝났지만 아이템을 지급하기 전에 함수가 실패한 경우에도, 같은 영수증을 다시 보내면 `UsedReceipt`가 반환됩니다.  
`details`의 아이템 정보로 다시 지급할 수 있지만, `UsedReceipt`를 받을 때마다 지급하면 같은 아이템이 여러 번 지급됩니다. 영수증별 지급 여부를 따로 기록해 이미 지급한 영수증에는 다시 지급하지 않도록 해 주세요.  
