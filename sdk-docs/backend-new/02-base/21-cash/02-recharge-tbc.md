---
sidebar_label: 영수증 검증을 통한 TBC 충전
description: "영수증 검증을 통한 TBC 충전"
---

# ChargeTBC

---
####
Android

public Task < ChargeTBCResult > **ChargeTBC**(string **receipt**, string **description**);

---
####
iOS

public Task < ChargeTBCResult > **ChargeTBC**(string **productId**, string **purchaseToken**, string **description**);


## 파라미터

### Android

| Value       | Type   | Description                                                |
| ----------- | ------ | ---------------------------------------------------------- |
| receipt | string | Purchasing.PurchaseEventArgs.purchasedProduct.receipt의 값 |
| description     | string | 추가로 저장하고자 하는 내용                                |

### iOS

| Value        | Type   | Description                  |
| ------------ | ------ | ---------------------------- |
| productId    | string | 구매한 product의 id 값       |
| purchaseToken | string | 구매 후 발급되는 영수증 토큰 |
| description      | string | 추가로 저장하고자 하는 내용  |

## 설명

결제한 영수증 토큰이 유효한지 확인 후 productId에 맞는 TBC(The Backend Coin)을 지급합니다.  

ChargeTBC 함수를 호출하기 위해서는 먼저 뒤끝 콘솔에 해당 productId(제품 ID)에 매칭되는 캐시를 등록해야 합니다.  
캐시를 등록하는 법에 대해서는 [콘솔 가이드-게임 캐시 관리](/guide/console-guide/backnd-base/cash)를 참고해 주세요.  

ChargeTBC 함수의 경우 Android와 iOS에서 호출하는 함수가 다릅니다.  
iOS의 경우 영수증 토큰에 productId가 포함되어 있지 않아 첫 번째 인자 값으로 productId를 기입해야 합니다.  

## Android Example

[유니티 IAP 서비스](https://docs.unity3d.com/Packages/com.unity.purchasing@4.12/manual/index.html)를 통해 획득한 receiptJson을 통한 영수증 검증

### Task 방식

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args)
{
    _ = ChargeTBC(args);

    return PurchaseProcessingResult.Complete;
}

public async Task ChargeTBC(Purchasing.PurchaseEventArgs args)
{
    var reqRequest = await BackndTBC.Instance.ChargeTBCAsync(args.purchasedProduct.receipt, "파격 할인 중!");
}
```

### Callback 방식

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args)
{
    BackndTBC.Instance.ChargeTBC(args.purchasedProduct.receipt, "파격 할인 중!", (callback) =>
    {
        // 이후 처리
    });
    return PurchaseProcessingResult.Complete;
}
```

## iOS Example

[유니티 IAP 서비스](https://docs.unity3d.com/Packages/com.unity.purchasing@4.12/manual/index.html)를 통해 획득한 영수증 토큰을 통한 영수증 검증

### 동기

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args)
{
    _ = ChargeTBC("pId", args);

    return PurchaseProcessingResult.Complete;
}

public async Task ChargeTBC(string productId, Purchasing.PurchaseEventArgs args)
{
    var reqRequest = await BackndTBC.Instance.ChargeTBCAsync(productId, args.purchasedProduct.receipt, "파격 할인 중!");
}
```

### 비동기

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args)
{
    BackndTBC.Instance.ChargeTBC("pId", args.purchasedProduct.receipt, "파격 할인 중!", (callback) =>
    {
        // 이후 처리
    });
    return PurchaseProcessingResult.Complete;
}
```

## ReturnCase

### Success cases

**충전에 성공한 경우**  
statusCode : 201  
message : Success  
returnValue : ReturnValueJson 참고

### Error cases

**유효하지 않은 영수증 토큰**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다

**환불/취소 영수증**  
statusCode : 402  
errorCode : AbnormalReceipt  
message : This receipt has changed status. purchaseState: cancelled

**뒤끝 콘솔에서 존재하지 않는 product를 구매한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : proudctId not found, proudctId을(를) 찾을 수 없습니다

**이미 사용한 영수증 토큰**  
statusCode : 409  
errorCode : UsedReceipt  
message : This receipt has already been used. usedDate: **2018-02-15T04:01:50.000Z**

## ReturnValueJson

```js
{
    usedTBC: 450, // 충전된 TBC 량
    amountTBC: 947.047 // 충전 이후 유저의 총 TBC 량
}
```
