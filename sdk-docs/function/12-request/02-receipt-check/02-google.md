---
sidebar_label: 구글 영수증 검증
description: "구글 영수증 검증"
---

# 구글 영수증 검증

구글 인앱결제 진행 후 발행된 영수증이 유효한지 확인하는 함수입니다.  
인앱결제 진행에 대한 자세한 설명은 뒤끝베이스의 [뒤끝펑션에서 인앱결제 검증하기 문서](/sdk-docs/backend/base/function/receipt-and-tbc/receipt-and-recharge-tbc)를 참고해 주세요.  

```js
IsValidateGooglePurchase(string productId, string token, string receiptDescription, bool isSubscription = false) -> BackendReturnObject

// example
var bro = Backend.Receipt.IsValidateGooglePurchase("pid", "token", "pid 영수증 검증", false);
```

### Parameters

| Value              | Type   | Description                                      |
| :----------------- | :----- | :----------------------------------------------- |
| productId          | string | 인앱결제를 진행한 제품의 ID                      |
| token              | string | 인앱결제 후 생성된 영수증 token 값               |
| receiptDescription | string | 영수증 검증 내역에서 추가로 확인하고자 하는 내용 |
| isSubscription     | bool   | 구독 결제 여부                                   |

### Return Cases

**성공한 경우(일반 상품)**  
statusCode : 201  
message : Success  
returnValue : {"usedDate":"2018-10-15T05:17:49Z"}

**성공한 경우(구독 상품)**  
statusCode : 200  
message : Success  
returnValue : 아래 참고

```js
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

> 3번 항목의 JWT 권한의 확인이 필요합니다.  
> 권한을 제대로 설정했는데도 bad token 에러가 발생할 경우 JWT를 재발급하고, 재발급 후에도 에러가 발생할 경우 JWT를 새로 생성한 후 다시 시도해야 합니다.  
> JWT 수정 여부는 바로 적용이 안될 수 있어 1~2시간 후에 다시 시도하는 것을 권장합니다.  
