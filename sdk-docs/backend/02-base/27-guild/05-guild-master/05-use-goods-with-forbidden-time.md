---
sidebar_label: "[Deprecated] 굿즈 사용(4시~5시 갱신불가)"
sidebar_position: "15"
draft: "true"
unlisted: "true"
description: "UseGoodsV4"
---

# UseGoodsV4

public BackendReturnObject **UseGoodsV4**(goodsType **type**, int **amount**);

:::info 안내
해당 함수는 랭킹 정산 에러를 방지하기 위해 매일 오전 4~5시외 시간에만 굿즈를 사용하는 함수입니다.  
 굿즈를 이용한 랭킹을 이용하지 않는 경우에는 제한이 존재하지 않는 ContributeGoodsV3를 이용하시기 바랍니다.  
:::

## 파라미터

| Value  | Type             | Description                                     |
| ------ | ---------------- | ----------------------------------------------- |
| type   | goodsType(enum) | 기부할 재화의 종류. goodsType.goodsN(N : 1~10) |
| amount | int              | 사용할 금액의 양. 음수만 허용.                  |

## 설명

길드의 해당 goodsType의 재화를 사용합니다.  

매일 오전 4~5시 사이에는 변경이 불가능하며, 요청 시 428에러가 리턴됩니다.  

UseGoodsV3와 사용법은 동일합니다.  

> 굿즈 정보를 이용한 랭킹 갱신 중 해당 함수의 호출이 발생할 경우, 낮은 확률로 랭킹 정산이 실패하게 됩니다. 정산 시간에는 갱신이 불가능하도록 해당 함수로 마이그레이션 하는 것을 권장드립니다.  

## Example

### 동기

```js
Backend.Guild.UseGoodsV4(goodsType.goods1, -20);
```

### 비동기

```js
Backend.Guild.UseGoodsV4(goodsType.goods1, -10, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.UseGoodsV4, goodsType.goods1, -10, (callback) => {
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

**새벽 4~5시 사이에 호출한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
