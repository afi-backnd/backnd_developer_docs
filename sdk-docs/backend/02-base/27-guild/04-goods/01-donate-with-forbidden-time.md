---
sidebar_label: "[Deprecated] 굿즈 기부(4시~5시 갱신불가)"
sidebar_position: "1.1"
draft: "true"
unlisted: "true"
description: "ContributeGoodsV4"
---

# ContributeGoodsV4

public BackendReturnObject **ContributeGoodsV4**(goodsType **type**, int **amount**);

:::info 안내
해당 함수는 랭킹 정산 에러를 방지하기 위해 매일 오전 4~5시외 시간에만 굿즈를 기부하는 함수입니다.  
 굿즈를 이용한 랭킹을 이용하지 않는 경우에는 제한이 존재하지 않는 ContributeGoodsV3를 이용하시기 바랍니다.  
:::

## 파라미터

| Value  | Type             | Description                                     |
| ------ | ---------------- | ----------------------------------------------- |
| type   | goodsType(enum) | 기부할 재화의 종류. goodsType.goodsN(N : 1~10) |
| amount | int              | 기부할 금액의 양. 양수만 허용.                  |

## 설명

현재 가입되어 있는 길드에 재화를 기부합니다.  

뒤끝은 열 가지 종류의 재화를 기부할 수 있도록 허용하고, 이 굿즈의 총량은 길드 생성 시 정할 수 있습니다.  
그리고 이를 각각 goods1, goods2, goods3 ⋯ goods10으로 명명합니다.  

매일 오전 4~5시 사이에는 변경이 불가능하며, 요청 시 428에러가 리턴됩니다.  

ContributeGoodsV3와 사용법은 동일합니다.  

> 굿즈 정보를 이용한 랭킹 갱신 중 해당 함수의 호출이 발생할 경우, 낮은 확률로 랭킹 정산이 실패하게 됩니다. 정산 시간에는 갱신이 불가능하도록 해당 함수로 마이그레이션 하는 것을 권장드립니다.  

## Example

### 동기

```js
Backend.Guild.ContributeGoodsV4(goodsType.goods1, 20);
```

### 비동기

```js
Backend.Guild.ContributeGoodsV4(goodsType.goods1, 10, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.ContributeGoodsV4, goodsType.goods1, 10, (callback) => {
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

**새벽 4~5시 사이에 호출한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted

