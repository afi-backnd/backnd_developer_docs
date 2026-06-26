---
sidebar_label: 영수증 검증
---

# ValidateReceipt

public BackendReturnObject **ValidateReceipt**(ReceiptParam **receiptParam**);  
public void **ValidateReceipt**(ReceiptParam **receiptParam**, Backend.BackendCallback<BackendReturnObject\> **callback**);

:::info Unity IAP 5.x 지원
이 API는 Unity IAP 5.x(Unity Purchasing) 환경을 지원합니다.
Unity IAP 4.x 환경을 사용하는 경우 [Legacy 영수증 검증](/sdk-docs/backend/base/receipt/receipt/android/verify) 문서를 참고해 주세요.

**Unity IAP 5.x 주요 변경 사항**
- `IStoreListener`/`ProcessPurchase` 콜백 방식이 `StoreController` 이벤트 방식으로 변경되었습니다.
- `Product.receipt` 속성이 deprecated 되었으며, `PendingOrder.Info.Receipt`로 영수증을 받아야 합니다.
- iOS에서는 StoreKit 2 기반의 JWS(JSON Web Signature) 형식 영수증(`jwsRepresentation`)을 사용합니다.
:::

:::danger 영수증 검증 이용 시 주의 사항
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

## ReceiptParam

`ReceiptParam`은 빌더 패턴으로 영수증 검증에 필요한 파라미터를 구성하는 클래스입니다.

### 생성자

| 생성자 | Description |
| ------ | ----------- |
| ReceiptParam(string **receipt**) | 통합 영수증 검증 <br/> 플랫폼(Android/iOS)에 따라 스토어 타입을 자동으로 결정합니다. |
| ReceiptParam(string **receiptType**, string **receipt**) | 수동 영수증 검증 <br/> 스토어 타입을 직접 지정합니다. (google, apple, onestore 등) |

### 메서드

| 메서드 | Description |
| ------ | ----------- |
| Subscription() | 구독 상품으로 설정합니다. |
| AddDetailItemId(string **itemId**) | 게임 내 아이템 ID를 추가합니다. <br/> 동일한 스토어 상품으로 여러 게임 아이템을 판매하는 경우 구분 용도로 사용합니다. |
| AddDetailItemName(string **itemName**) | 게임 내 아이템 이름을 추가합니다. |
| AddDetail(string **key**, string **value**) | 상세 정보에 커스텀 key-value를 추가합니다. |
| SetProductId(string **productId**) | 상품 ID를 설정합니다. <br/> 수동 모드에서만 사용됩니다. |
| SetGamerId(string **gamerId**) | 뒤끝 로그인 없이 영수증 검증 시 게이머 ID를 설정합니다. |
| SetIsGlobal(bool **isGlobal**) | ONE Store Global 플랫폼 여부를 설정합니다. <br/> Google Play, iOS에서는 무시됩니다. |
| SetIapPrice(string **iapPrice**, string **iapCurrency**) | 인앱 구매 금액과 통화를 설정합니다. <br/> ONE Store 등 기타 스토어에서는 필수입니다. (Google Play, iOS는 서버에서 자동 조회) |

:::info 통합 모드와 수동 모드
- **통합 모드** `ReceiptParam(receipt)`: Android에서는 `google`, iOS에서는 `apple`로 자동 결정됩니다. **Android와 iOS에서만 사용 가능하며**, 다른 플랫폼에서는 예외가 발생합니다. 통합 모드에서는 `SetProductId`, `SetIsGlobal`, `SetIapPrice`가 무시됩니다.
- **수동 모드** `ReceiptParam(receiptType, receipt)`: `google`, `apple`, `onestore` 등 스토어 타입을 직접 지정합니다. ONE Store 등 추가 스토어를 사용하거나, `SetProductId`, `SetIapPrice` 등 추가 파라미터를 설정하려면 수동 모드를 사용해야 합니다.
:::

## 설명

Unity IAP 5.x(Unity Purchasing)에서 구매한 상품에 대한 영수증을 받아 뒤끝 서버를 통해 영수증 검증을 받습니다.

- 뒤끝은 **영수증 자체의 유효성과, 구매한 productId**를 검증합니다.
- 기본적으로 뒤끝 로그인이 필요합니다. 로그인 없이 검증하려면 `SetGamerId`를 사용하세요.
- `AddDetail`로 추가한 상세 정보는 뒤끝 콘솔의 영수증 검증 항목에 함께 저장됩니다.

> 영수증 검증을 위해서는 플랫폼별 콘솔 설정이 필요합니다.
> - 구글: [구글 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/android/setting-google-console)
> - 애플: [애플 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/ios/setting-google-console)
> - 원스토어: [원스토어 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/onestore/setting-onestore-console)

### iOS 영수증 처리

Unity IAP 5.x는 iOS에서 StoreKit 2를 사용하며, 영수증이 기존 App Receipt 형식 대신 JWS(JSON Web Signature) 형식으로 제공됩니다.
iOS에서는 `PendingOrder.Info.Receipt` 대신 `PendingOrder.Info.Apple.jwsRepresentation`을 사용해야 합니다.

```js
string receipt = pendingOrder.Info.Receipt;

// iOS에서는 StoreKit 2 기반의 JWS 영수증을 사용
if (Application.platform == RuntimePlatform.IPhonePlayer || Application.platform == RuntimePlatform.OSXPlayer)
{
    receipt = pendingOrder.Info.Apple.jwsRepresentation;
}
```

### 금액 표시 (SetIapPrice)

뒤끝 콘솔의 영수증 검증 항목에서 해당 구매 내역의 금액을 표시합니다.

- **Google Play, iOS**: 서버에서 금액 정보를 자동으로 조회하여 콘솔에 표시하므로 설정이 필요 없습니다.
- **ONE Store 등 기타 스토어**: 서버에서 금액 정보를 조회할 수 없으므로, `SetIapPrice`를 통해 SDK에서 직접 전달해야 합니다. **(필수)**

```js
var param = new ReceiptParam("onestore", receipt)
    .SetProductId(productId)
    .SetIapPrice(iapPrice, iapCurrency);
```

## Example

### 통합 모드 (Google Play / iOS 자동 판별)

#### 동기

```js
// Unity IAP 5.x - PendingOrder에서 영수증 추출
string receipt = pendingOrder.Info.Receipt;

if (Application.platform == RuntimePlatform.IPhonePlayer || Application.platform == RuntimePlatform.OSXPlayer)
{
    receipt = pendingOrder.Info.Apple.jwsRepresentation;
}

// ReceiptParam 구성
// AddDetailItemId/AddDetailItemName은 게임 내 아이템 구분 용도
var param = new ReceiptParam(receipt)
    .AddDetailItemId("10001")
    .AddDetailItemName("골드 100개");

// 영수증 검증
BackendReturnObject bro = Backend.Receipt.ValidateReceipt(param);

if (bro.IsSuccess())
{
    Debug.Log("영수증 검증 성공");
    // 구매 성공한 제품에 대한 보상 지급
}
else
{
    Debug.LogError("영수증 검증 실패: " + bro.GetMessage());
}
```

#### 비동기

```js
string receipt = pendingOrder.Info.Receipt;

if (Application.platform == RuntimePlatform.IPhonePlayer || Application.platform == RuntimePlatform.OSXPlayer)
{
    receipt = pendingOrder.Info.Apple.jwsRepresentation;
}

var param = new ReceiptParam(receipt)
    .AddDetailItemId("10001")
    .AddDetailItemName("골드 100개");

Backend.Receipt.ValidateReceipt(param, bro =>
{
    if (bro.IsSuccess())
    {
        Debug.Log("영수증 검증 성공");
        // 구매 성공한 제품에 대한 보상 지급
    }
    else
    {
        Debug.LogError("영수증 검증 실패: " + bro.GetMessage());
    }
});
```

### 구독 상품

```js
var param = new ReceiptParam(receipt)
    .Subscription()
    .AddDetailItemId("20001");

BackendReturnObject bro = Backend.Receipt.ValidateReceipt(param);
```

### 수동 모드 (스토어 타입 직접 지정)

#### ONE Store

```js
var param = new ReceiptParam("onestore", purchaseToken)
    .SetProductId(productId)
    .SetIsGlobal(false)
    .SetIapPrice(iapPrice, iapCurrency)
    .AddDetailItemId("10001")
    .AddDetailItemName("골드 100개");

BackendReturnObject bro = Backend.Receipt.ValidateReceipt(param);

if (bro.IsSuccess())
{
    Debug.Log("원스토어 영수증 검증 성공");
}
else
{
    Debug.LogError("원스토어 영수증 검증 실패: " + bro.GetMessage());
}
```

#### ONE Store Global

```js
var param = new ReceiptParam("onestore", purchaseToken)
    .SetProductId(productId)
    .SetIsGlobal(true)
    .SetIapPrice(iapPrice, iapCurrency);

BackendReturnObject bro = Backend.Receipt.ValidateReceipt(param);
```

### 비로그인 영수증 검증

뒤끝 로그인 없이 영수증을 검증하려면 `SetGamerId`를 사용합니다.

```js
var param = new ReceiptParam(receipt)
    .SetGamerId("게이머ID")
    .AddDetailItemId("10001");

BackendReturnObject bro = Backend.Receipt.ValidateReceipt(param);
```

### 게임 아이템 상세 정보 추가

동일한 스토어 상품(productId)으로 여러 게임 아이템을 판매하는 경우, `AddDetailItemId`와 `AddDetailItemName`으로 게임 내 아이템을 구분할 수 있습니다.
스토어 상품 ID(productId)는 영수증에 이미 포함되어 있으므로 별도로 추가할 필요가 없습니다.

```js
// 예: 스토어 상품 "gem_pack"으로 여러 게임 아이템을 판매하는 경우
var param = new ReceiptParam(receipt)
    .AddDetailItemId("10002")                    // 게임 내 아이템 ID
    .AddDetailItemName("빨간 보석 50개")            // 게임 내 아이템 이름
    .AddDetail("iconAddress", "icon_gem_red")
    .AddDetail("category", "consumable");

BackendReturnObject bro = Backend.Receipt.ValidateReceipt(param);
```

## ReturnCase

### Success cases

| statusCode | message | returnValue |
| :--------: | ------- | ----------- |
| 200 | Success | {"usedDate":"2018-10-15T05:17:49Z"} |

### Error cases

#### 공통 에러

| statusCode | errorCode | 설명 |
| :--------: | --------- | ---- |
| 400 | UndefinedParameterException | 콘솔에 원스토어 정보가 올바르지 않은 경우 <br/> undefined onestore client_id, onestore client_id을(를) 확인할 수 없습니다 |
| 400 | BadParameterException | 영수증 토큰이 비어있을 경우 <br/> undefined token, token을(를) 확인할 수 없습니다 |
| 400 | BadParameterException | productId가 비어있을 경우 (Google Play/iOS 제외) <br/> undefined productId, productId을(를) 확인할 수 없습니다 |
| 402 | AbnormalReceipt | 환불/취소된 영수증 <br/> This receipt has changed status. purchaseState: cancelled |
| 409 | UsedReceipt | 이미 사용한 영수증 토큰 <br/> This receipt has already been used. usedDate: 2018-02-15T04:01:50.000Z |

#### 플랫폼별 에러

영수증 토큰 검증 시 플랫폼별로 `details` 필드가 포함된 응답이 반환됩니다. `details`를 통해 에러의 구체적인 원인을 확인할 수 있습니다.

##### Android

**유효하지 않은 영수증 토큰**

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

**발급받은 구글 JWT의 권한이 없는 경우**

> 구글 콘솔쪽 권한 부여가 진행 중이거나, 잘못 설정한 경우가 많습니다.

```json
{
    "statusCode": 400,
    "errorCode": "BadParameterException",
    "message": "bad token, 잘못된 token 입니다",
    "errorData": {
        "errorCode": 401,
        "apiError": null,
        "errorType": "PERMISSION_DENIED",
        "message": "The current user has insufficient permissions to perform the requested operation."
    }
}
```

:::info bad token 에러 발생 시
JWT 권한의 확인이 필요합니다.
권한을 제대로 설정했는데도 bad token 에러가 발생할 경우 JWT를 재발급하고, 재발급 후에도 에러가 발생할 경우 JWT를 새로 생성한 후 다시 시도해야 합니다.
JWT 수정 여부는 바로 적용이 안될 수 있어 1~2시간 후에 다시 시도하는 것을 권장합니다.
:::

##### iOS

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

##### ONE Store

**영수증 검증이 유효하지 않을 경우 / 위변조된 영수증 토큰**

> product ID가 올바르지 않은 경우에도 동일하게 반환합니다.

```json
{
    "statusCode": 400,
    "errorCode": "BadParameterException",
    "message": "bad token, 잘못된 token 입니다",
    "errorData": {
        "errorCode": 404,
        "apiError": null,
        "errorType": "NoSuchData",
        "message": "The requested data could not be found."
    }
}
```

**clientId 또는 clientSecret이 올바르지 않은 경우**

```json
{
    "statusCode": 400,
    "errorCode": "BadParameterException",
    "message": "bad token, 잘못된 token 입니다",
    "errorData": {
        "errorCode": 400,
        "apiError": null,
        "errorType": "OauthBadRequest",
        "message": "Unknown client or wrong secret."
    }
}
```

### UsedReceipt 응답 상세

`UsedReceipt`(409) 에러가 반환될 때, 응답에는 기존 검증 시 저장된 `details` 정보가 함께 포함됩니다.

예를 들어 `AddDetailItemId("10001")`, `AddDetailItemName("골드 100개")`로 검증한 영수증에 대해 `UsedReceipt`가 반환되면 다음과 같은 응답을 받습니다.

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

| 필드 | 설명 |
| ---- | ---- |
| details.receiptInfo.inDate | 최초 영수증 검증 시각 |
| details.receiptInfo.details | 최초 검증 시 `AddDetail`로 저장한 상세 정보 <br/> (itemId, itemName 등이 JSON string으로 포함) |

### UsedReceipt를 활용한 상품 재지급

영수증 검증은 완료되었지만 상품 지급 전에 앱이 종료된 경우(앱 크래시, 네트워크 끊김 등), 앱 재시작 시 동일한 영수증으로 다시 검증을 요청하면 `UsedReceipt`가 반환됩니다.

이때 응답의 `details.receiptInfo.details`에 포함된 게임 아이템 정보(`itemId`, `itemName`)를 파싱하여 **미지급된 상품을 재지급**할 수 있습니다.

```js
Backend.Receipt.ValidateReceipt(param, bro =>
{
    var errorCode = bro.GetErrorCode();

    if (bro.IsSuccess())
    {
        // 최초 검증 성공 → 상품 지급 + 구매 확정
        GrantItem(shopItemId);
        _storeController.ConfirmPurchase(pendingOrder);
    }
    else if (errorCode == "UsedReceipt")
    {
        // 이미 검증된 영수증 → details에서 아이템 정보를 확인하여 상품 재지급
        var returnValue = bro.GetReturnValuetoJSON();
        var detailsJson = returnValue["details"]["receiptInfo"]["details"].ToString();

        // details를 파싱하여 게임 아이템 ID와 이름 추출
        var detailData = JsonMapper.ToObject(detailsJson);
        string itemId = detailData["itemId"].ToString();       // "10001"
        string itemName = detailData["itemName"].ToString();   // "골드 100개"

        Debug.Log($"미지급 상품 재지급: {itemId} ({itemName})");

        GrantItem(itemId);
        _storeController.ConfirmPurchase(pendingOrder);
    }
    else
    {
        // 검증 실패
        Debug.LogError($"영수증 검증 실패: ({errorCode}) {bro.GetMessage()}");
    }
});
```

:::danger 상품 재지급 시 중복 지급 방지
`UsedReceipt` 응답을 받았다고 무조건 상품을 지급하면 중복 지급이 발생할 수 있습니다.
뒤끝의 [게임 정보](/sdk-docs/backend/base/game-information/insert) 또는 [유저 데이터](/sdk-docs/backend/base/player-data/insert) 기능을 활용하여 해당 영수증에 대한 상품 지급 여부를 별도로 관리하고 중복 지급을 방지해야 합니다.
:::
