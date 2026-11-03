---
sidebar_label: "[Legacy] 애플 영수증 검증"
description: "애플 영수증 검증 (Unity IAP 4.x)"
---

# 애플 영수증 검증

:::info Unity IAP 4.x용 이전 API
이 문서는 Unity IAP 4.x 환경에서 사용하던 이전 API입니다.  
Unity IAP 5.x를 사용한다면 [영수증 검증](/sdk-docs/function/backnd-features/receipt-check/validate-receipt) 문서를 참고해 주세요.  
:::

애플 인앱 결제 후 발행된 영수증이 유효한지 확인하는 함수입니다.  
인앱 결제를 진행하는 방법은 뒤끝베이스의 [영수증 검증/TBC 충전](/sdk-docs/backend/base/function/receipt-and-tbc/receipt-and-recharge-tbc) 문서를 참고해 주세요.  

```csharp
IsValidateApplePurchase(string receiptJson, string receiptDescription) -> BackendReturnObject

// example
var bro = Backend.Receipt.IsValidateApplePurchase("receiptJson", "영수증 검증");
```

## 파라미터

| Value              | Type   | Description                                      |
| :----------------- | :----- | :----------------------------------------------- |
| receiptJson        | string | 인앱 결제 후 생성된 영수증 토큰                  |
| receiptDescription | string | 영수증 검증 내역에서 추가로 확인할 내용          |

## ReturnCase

**성공한 경우**  
statusCode : 201  
message : Success  
returnValue : {"usedDate":"2018-10-15T05:17:49Z"}

**환불/취소 영수증**  
statusCode : 402  
errorCode : AbnormalReceipt  
message : This receipt has changed status. purchaseState: cancelled

**이미 사용한 영수증 토큰**  
statusCode : 409  
errorCode : UsedReceipt  
message : This receipt has already been used. usedDate: 2018-02-15T04:01:50.000Z

**유효하지 않은 영수증 토큰**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다
