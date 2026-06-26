---
sidebar_label: TBC로 아이템 구매
---

# UseTBC

public BackendReturnObject **UseTBC**(string **uuid**);  
public BackendReturnObject **UseTBC**(string **uuid**, string **details**);

:::warning TBC 기능 제공 중단 안내
스토어 영수증 검증 API의 최신화에 따라 기존 방식과의 호환성 문제 및 기능 전반에 대한 개선 필요 사항이 확인되어, 기능 제공이 중단되었습니다.

**SDK 5.18.7 이하 버전에서는 기존과 동일하게 TBC 기능을 계속 이용하실 수 있습니다.**
:::

## 파라미터

| Value   | Type   | Description                                                                 |
| :------ | :----- | :-------------------------------------------------------------------------- |
| uuid    | string | GetProductList의 BRO Return Value > JsonArray >(JsonObject) inDate > value |
| details | string | (Optional) TBC 사용내역에 추가로 저장할 정보                                |

## 설명

게임 아이템의 uuid를 통해 해당 아이템을 구매합니다.  
게임 아이템의 uuid는 `TBC.GetProductList()`를 통해서 획득할 수 있습니다.  
게임 유저의 TBC는 해당 아이템의 TBC 만큼 차감됩니다.  

## Example

### 동기

```js
string uuid = Backend.TBC.GetProductList().Rows()[0]["uuid"]["S"].ToString();
Backend.TBC.UseTBC(uuid);
Backend.TBC.UseTBC(uuid , "장검 구매");
```

### 비동기

```js
Backend.TBC.GetProductList((callback) =>
{
     string uuid = callback.Rows()[0]["uuid"]["S"].ToString();

     Backend.TBC.UseTBC(uuid, (callback) =>
     {
    // 이후 처리
     });

     Backend.TBC.UseTBC(uuid , "장검 구매", (callback) =>
     {
    // 이후 처리
     });
}
```

### SendQueue

```js
SendQueue.Enqueue(Backend.TBC.GetProductList, (callback) =>
{
     string uuid = callback.Rows()[0]["uuid"]["S"].ToString();

     SendQueue.Enqueue(Backend.TBC.UseTBC, uuid, (callback) =>
     {
    // 이후 처리
     });

     SendQueue.Enqueue(Backend.TBC.UseTBC, uuid , "장검 구매", (callback) =>
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
