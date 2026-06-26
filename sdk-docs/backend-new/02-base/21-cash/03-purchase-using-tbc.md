---
sidebar_label: TBC로 아이템 구매
description: "TBC로 아이템 구매"
---

# BuyProduct

public Task< BuyProductResult > **BuyProductAsync**(string **uuid**);  
public Task< BuyProductResult > **BuyProductAsync**(string **uuid**, string **details**);

## 파라미터

| Value   | Type   | Description                                                                 |
| :------ | :----- | :-------------------------------------------------------------------------- |
| uuid    | string | GetProducts의 GetProductsResult > GetInfoList > infoList[i].Uuid |
| details | string | (Optional) TBC 사용내역에 추가로 저장할 정보                                |

## 설명

게임 아이템의 uuid를 통해 해당 아이템을 구매합니다.  
게임 아이템의 uuid는 `TBC.GetProducts()`를 통해서 획득할 수 있습니다.  
게임 유저의 TBC는 해당 아이템의 TBC 만큼 차감됩니다.  

## Example

### Task 방식

```js
var reqResult = await BackndTBC.Instance.GetProductsAsync();
var uuid = reqResult.GetInfoList()[0].Uuid;
var reqResult2 = await BackndTBC.Instance.BuyProductAsync(uuid);
var reqResult3 = await BackndTBC.Instance.BuyProductAsync(uuid, "장검 구매");
```

### Callback 방식

```js
BackndTBC.Instance.GetProducts((callback) =>
{
     string uuid = callback.GetInfoList()[0].Uuid;

     BackndTBC.Instance.BuyProduct(uuid, (callback) =>
     {
          // 이후 처리
     });

     BackndTBC.Instance.BuyProduct(uuid , "장검 구매", (callback) =>
     {
          // 이후 처리
     });
}
```

## ReturnCase

### Success cases

**아이템 구매에 성공한 경우**  
statusCode : 201  
message : Success  
returnValue : `{ usedTBC: 450, amountTBC: 947.047 }`

### Error cases

**uuid에 해당하는 캐시 아이템을 사는데 TBC가 부족한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad usedTBC, 잘못된 usedTBC 입니다

**존재하지 않는 아이템을 구매 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : proudctId not found, proudctId을(를) 찾을 수 없습니다
