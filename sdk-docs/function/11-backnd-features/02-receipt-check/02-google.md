---
sidebar_label: "[Legacy] 구글 영수증 검증"
description: "구글 영수증 검증 (Unity IAP 4.x)"
---

# 구글 영수증 검증

:::info Unity IAP 4.x용 이전 API
이 문서는 Unity IAP 4.x 환경에서 사용하던 이전 API입니다.  
Unity IAP 5.x를 사용한다면 [영수증 검증](/sdk-docs/function/backnd-features/receipt-check/validate-receipt) 문서를 참고해 주세요.  
:::

구글 인앱 결제 후 발행된 영수증이 유효한지 확인하는 함수입니다.  
인앱 결제를 진행하는 방법은 뒤끝베이스의 [영수증 검증/TBC 충전](/sdk-docs/backend/base/function/receipt-and-tbc/receipt-and-recharge-tbc) 문서를 참고해 주세요.  

```csharp
IsValidateGooglePurchase(string productId, string token, string receiptDescription, bool isSubscription = false) -> BackendReturnObject

// example
var bro = Backend.Receipt.IsValidateGooglePurchase("pid", "token", "pid 영수증 검증", false);
```

## 파라미터

| Value              | Type   | Description                                      |
| :----------------- | :----- | :----------------------------------------------- |
| productId          | string | 인앱 결제한 제품의 ID                            |
| token              | string | 인앱 결제 후 생성된 영수증 토큰 값               |
| receiptDescription | string | 영수증 검증 내역에서 추가로 확인할 내용          |
| isSubscription     | bool   | 구독 결제 여부                                   |

## ReturnCase

**성공한 경우(일반 상품)**  
statusCode : 201  
message : Success  
returnValue : {"usedDate":"2018-10-15T05:17:49Z"}

**성공한 경우(구독 상품)**  
statusCode : 200  
message : Success  
returnValue : 아래 참고

```json
{
  "kind":"androidpublisher#subscriptionPurchase",
  "startTimeMillis":"1583722491833",
  "expiryTimeMillis":"1583722908247",
  "autoRenewing":true,
  "priceCurrencyCode":"KRW",
  "priceAmountMicros":"1000000000",
  "countryCode":"KR",
  "developerPayload":"{\"developerPayload\":\"\",\"is_free_trial\":false,\"has_introductory_price_trial\":false,\"is_updated\":false}",
  "paymentState":1,
  "orderId":"주문 Id",
  "purchaseType":0,
  "acknowledgementState":1
}
```

**이미 사용한 영수증 토큰**  
statusCode : 409  
errorCode : UsedReceipt  
message : This receipt has already been used. usedDate: 2018-02-15T04:01:50.000Z

**이미 사용하거나 취소된 구독 상품의 영수증을 검증한 경우**  
statusCode : 409  
errorCode : DuplicatedParameterException  
message : Duplicated receipt, 중복된 receipt 입니다

**환불/취소된 영수증**  
statusCode : 402  
errorCode : AbnormalReceipt  
message : This receipt has changed status. purchaseState: cancelled

**유효하지 않은 영수증 토큰**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다

> bad token 에러가 발생하면 JWT 권한을 확인해 주세요.  
> 권한을 제대로 설정했는데도 bad token 에러가 발생하면 JWT를 재발급하고, 재발급한 뒤에도 에러가 발생하면 JWT를 새로 생성한 뒤 다시 시도해 주세요.  
> JWT를 수정한 내용은 바로 적용되지 않을 수 있으므로 1~2시간 후에 다시 시도하는 것을 권장합니다.  
