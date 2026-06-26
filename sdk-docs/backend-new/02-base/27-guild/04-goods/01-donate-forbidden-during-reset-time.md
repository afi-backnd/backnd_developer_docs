---
sidebar_label: "[New] 굿즈 기부(리더 보드 초기화 시간 갱신 불가)"
---

# DonateGoods

public Task< RequestResult > **DonateGoodsAsync**(GoodsType **type**, int **amount**);

:::info 안내
해당 함수는 랭킹 정산 에러를 방지하기 위해 리더보드 정산 시간을 제외한 시간에만 굿즈를 기부하는 함수입니다.  
:::

## 파라미터

| Value  | Type             | Description                                     |
| ------ | ---------------- | ----------------------------------------------- |
| type   | GoodsType(enum)  | 기부할 재화의 종류. GoodsType.goodsN(N : 1~10) |
| amount | int              | 기부할 금액의 양. 양수만 허용.                  |

## 설명

현재 가입되어 있는 길드에 재화를 기부합니다.  

뒤끝은 열 가지 종류의 재화를 기부할 수 있도록 허용하고, 이 굿즈의 총량은 길드 생성 시 정할 수 있습니다.  
그리고 이를 각각 goods1, goods2, goods3 ⋯ goods10으로 명명합니다.  

리더보드에 초기화 주기가 설정 되어 있다면 리더보드 초기화 시간에는 변경이 불가능하며, 요청 시 428에러가 리턴됩니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.DonateGoodsAsync(GoodsType.goods1, 20);
```

### Callback 방식

```js
BackndGuild.Instance.DonateGoods(GoodsType.goods1, 10, (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**기부에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**goodsCount 이상의 goodsType에 기부한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad goodsType, 잘못된 goodsType 입니다

**amount 가 음수인 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : type 사전 조건을 만족하지 않습니다.  

**리더보드 정산 시간 사이에 호출한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted

