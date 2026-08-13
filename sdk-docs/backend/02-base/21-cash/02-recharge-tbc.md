---
sidebar_label: "영수증 검증을 통한 TBC 충전"
description: "ChargeTBC"
---

# ChargeTBC

#### Android

public BackendReturnObject **ChargeTBC**(string **receiptJson**, string **details**);

---
#### iOS

public BackendReturnObject **ChargeTBC**(string **productId**, string **receiptToken**, string **details**);

---
#### OneStore

public BackendReturnObject **ChargeTBCByOneStore**(bool **isGlobal**, string **productId**, string **receiptToken**, string **details**);

:::warning TBC 기능 제공 중단 안내
스토어 영수증 검증 API의 최신화에 따라 기존 방식과의 호환성 문제 및 기능 전반에 대한 개선 필요 사항이 확인되어, 기능 제공이 중단되었습니다.

**SDK 5.18.7 이하 버전에서는 기존과 동일하게 TBC 기능을 계속 이용하실 수 있습니다.**
:::

## 파라미터

### Android

| Value       | Type   | Description                                                |
| ----------- | ------ | ---------------------------------------------------------- |
| receiptJson | string | Purchasing.PurchaseEventArgs.purchasedProduct.receipt의 값 |
| details     | string | 추가로 저장하고자 하는 내용                                |

### iOS

| Value        | Type   | Description                  |
| ------------ | ------ | ---------------------------- |
| productId    | string | 구매한 product의 id 값       |
| receiptToken | string | 구매 후 발급되는 영수증 토큰 |
| details      | string | 추가로 저장하고자 하는 내용  |

### OneStore
| Value        | Type   | Description                                          |
| ------------ | ------ | ---------------------------------------------------- |
| isGlobal     | bool   | 글로벌 앱 여부 (true: 글로벌, false: 국내 전용)        |
| productId    | string | 구매한 product의 id 값                               |
| receiptToken | string | 구매 후 발급되는 영수증 토큰                         |
| details      | string | 추가로 저장하고자 하는 내용                          |

## 설명

결제한 영수증 토큰이 유효한지 확인 후 productId에 맞는 TBC(The Backend Coin)을 지급합니다.  

ChargeTBC 함수를 호출하기 위해서는 먼저 뒤끝 콘솔에 해당 productId(제품 ID)에 매칭되는 캐시를 등록해야 합니다.  
캐시를 등록하는 법에 대해서는 [콘솔 가이드-게임 캐시 관리](/guide/console-guide/backnd-base/cash)를 참고해 주세요.  

ChargeTBC 함수의 경우 Android와 iOS, OneStore에서 호출하는 함수가 다릅니다.  
iOS와 OneStore 경우 영수증 토큰에 productId가 포함되어 있지 않아 첫 번째 인자 값으로 productId를 기입해야 합니다.  

## Android Example

[유니티 IAP 서비스](https://docs.unity3d.com/Packages/com.unity.purchasing@4.12/manual/index.html)를 통해 획득한 receiptJson을 통한 영수증 검증

### 동기

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args){
    Backend.TBC.ChargeTBC(args.purchasedProduct.receipt, "파격 할인 중!");
    return PurchaseProcessingResult.Complete;
}

```

### 비동기

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args)
{
    Backend.TBC.ChargeTBC(args.purchasedProduct.receipt, "파격 할인 중!", (callback) =>
    {
        // 이후 처리
    });
    return PurchaseProcessingResult.Complete;
}
```

### SendQueue

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args)
{
    SendQueue.Enqueue(Backend.TBC.ChargeTBC, args.purchasedProduct.receipt, "파격 할인 중!", callback  =>
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
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args){
    Backend.TBC.ChargeTBC("pId", args.purchasedProduct.receipt, "파격 할인 중!");
    return PurchaseProcessingResult.Complete;
}
```

### 비동기

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args)
{
    Backend.TBC.ChargeTBC("pId", args.purchasedProduct.receipt, "파격 할인 중!", (callback) =>
    {
        // 이후 처리
    });
    return PurchaseProcessingResult.Complete;
}
```

### SendQueue

```js
public Purchasing.PurchaseProcessingResult ProcessPurchase(Purchasing.PurchaseEventArgs args)
{
    SendQueue.Enqueue(Backend.TBC.ChargeTBC, "pId", args.purchasedProduct.receipt, "파격 할인 중!", callback  =>
    {
        // 이후 처리
    });
    return PurchaseProcessingResult.Complete;
}
```

## OneStore Example

[원스토어 영수증 검증](/sdk-docs/backend/base/receipt/onestore/verify) 문서를 참고하여 영수증 토큰을 획득한 후 ChargeTBCByOneStore 함수를 호출합니다.

### 동기

```js
public void OnConsumeSucceeded(PurchaseData purchase)
{
    var bro = Backend.TBC.ChargeTBCByOneStore(false, "pId", purchase.Receipt, "파격 할인 중!");
    if (bro.IsSuccess())
    {
        // 이후 처리
    }
}
```

### 비동기
```js
public void OnConsumeSucceeded(PurchaseData purchase)
{
    Backend.TBC.ChargeTBCByOneStore(false, "pId", purchase.Receipt, "파격 할인 중!", (callback) =>
    {
        // 이후 처리
    });
}
```

### SendQueue

```js
public void OnConsumeSucceeded(PurchaseData purchase)
{
    SendQueue.Enqueue(Backend.TBC.ChargeTBCByOneStore, false, "pId", purchase.Receipt, "파격 할인 중!", (callback) =>
    {
        // 이후 처리
    });
}
```

## ReturnCase

### Success cases

**충전에 성공한 경우**  
statusCode : 201  
message : Success  
returnValue : GetReturnValuetoJSON 참고

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

## GetReturnValuetoJSON

```js
{
    usedTBC: 450, // 충전된 TBC 량
    amountTBC: 947.047 // 충전 이후 유저의 총 TBC 량
}
```
