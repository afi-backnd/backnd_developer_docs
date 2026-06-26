---
sidebar_label: 애플 영수증 검증
---

# 애플 영수증 검증

애플 인앱결제 진행 후 발행된 영수증이 유효한지 확인하는 함수입니다.  
인앱결제 진행에 대한 자세한 설명은 뒤끝베이스의 [뒤끝펑션에서 인앱결제 검증하기 문서](/sdk-docs/backend/base/function/receipt-and-tbc/receipt-and-recharge-tbc)를 참고해 주세요.  

```js
IsValidateApplePurchase(string receiptJson, string receiptDescription) -> BackendReturnObject

// example
var bro = Backend.Receipt.IsValidateApplePurchase("receiptJson", "영수증 검증");
```

### Parameters

| Value              | Type   | Description                                      |
| :----------------- | :----- | :----------------------------------------------- |
| receiptJson        | string | 인앱 결제 후 생성된 영수증 토큰                  |
| receiptDescription | string | 영수증 검증 내역에서 추가로 확인하고자 하는 내용 |

### Return Cases

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
