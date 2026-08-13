---
sidebar_label: "[Deprecated] 굿즈 사용"
sidebar_position: "14"
draft: "true"
unlisted: "true"
description: "UseGoodsV3"
---

# UseGoodsV3

public BackendReturnObject **UseGoodsV3**(goodsType **type**, int **amount**);

:::danger 랭킹 정산 도중 함수 호출 시 랭킹 정산 실패 문제
굿즈 정보를 이용한 길드 랭킹을 이용 시, **랭킹 정산 시간에 해당 함수를 호출할 경우 랭킹 정산이 비정상적으로 종료**될 수 있습니다.  

따라서 굿즈 정보를 이용한 길드 랭킹을 이용하는 경우, 4~5시에 호출에는 갱신이 불가능한  **UseGoodsV4** 기능을 이용해주시기 바랍니다.  
:::

## 파라미터

| Value  | Type             | Description                                     |
| ------ | ---------------- | ----------------------------------------------- |
| type   | goodsType(enum) | 기부할 재화의 종류. goodsType.goodsN(N : 1~10) |
| amount | int              | 사용할 금액의 양. 음수만 허용.                  |

## 설명

길드의 해당 goodsType의 재화를 사용합니다.  

## Example

### 동기

```js
Backend.Guild.UseGoodsV3(goodsType.goods1, -20);
```

### 비동기

```js
Backend.Guild.UseGoodsV3(goodsType.goods1, -10, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.UseGoodsV3, goodsType.goods1, -10, (callback) => {
    // 이후 처리
  });
```

## ReturnCase

### Success cases

**사용한 경우**  
statusCode : 204  
message : Success  

### Error cases

**마스터 이외의 길드원이 사용 시도한 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden useGoods, 금지된 useGoods

**amount 가 양수인 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : type 사전 조건을 만족하지 않습니다.  

**사용량이 보유량보다 큰 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : inadequateAmount 사전 조건을 만족하지 않습니다.  

**길드에 속해있지 않은 사람이 사용 시도한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : notGuildMember 사전 조건을 만족하지 않습니다.  
