---
sidebar_label: "굿즈 기부(리더 보드 초기화 시간 갱신 불가)"
description: "ContributeGoodsV5"
---

# ContributeGoodsV5

public BackendReturnObject **ContributeGoodsV5**(goodsType **type**, int **amount**);  
public BackendReturnObject **ContributeGoodsV5**(goodsType **type**, double **amount**);

:::info 안내
해당 함수는 랭킹 정산 에러를 방지하기 위해 리더보드 정산 시간을 제외한 시간에만 굿즈를 기부하는 함수입니다.  
기존 ContributeGoodsV3, ContributeGoodsV4 함수의 개선된 기능을 제공합니다.  
:::

## 파라미터

| Value  | Type             | Description                                     |
| ------ | ---------------- | ----------------------------------------------- |
| type   | goodsType(enum) | 기부할 재화의 종류. goodsType.goodsN(N : 1~10) |
| amount | int, double      | 기부할 금액의 양. 양수만 허용.                  |

## 설명

현재 가입되어 있는 길드에 재화를 기부합니다.  

뒤끝은 열 가지 종류의 재화를 기부할 수 있도록 허용하고, 이 굿즈의 총량은 길드 생성 시 정할 수 있습니다.  
그리고 이를 각각 goods1, goods2, goods3 ⋯ goods10으로 명명합니다.  

리더보드에 초기화 주기가 설정 되어 있다면 리더보드 초기화 시간에는 변경이 불가능하며, 요청 시 428에러가 리턴됩니다.  

ContributeGoodsV3, ContributeGoodsV4와 사용법은 동일합니다.  

SDK 5.18.13부터 double 타입 amount를 사용할 수 있습니다.  
int 범위를 초과하는 큰 수치의 재화를 기부할 때 사용해 주세요. amount는 정수 값으로 처리되므로 소수점 이하 값은 버림 처리될 수 있습니다.  

## Example

### 동기

```js
Backend.Guild.ContributeGoodsV5(goodsType.goods1, 20);

// int 범위를 초과하는 값은 double 타입으로 전달할 수 있습니다.
Backend.Guild.ContributeGoodsV5(goodsType.goods1, 5000000000d);
```

### 비동기

```js
Backend.Guild.ContributeGoodsV5(goodsType.goods1, 10, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.ContributeGoodsV5, goodsType.goods1, 10, (callback) => {
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

