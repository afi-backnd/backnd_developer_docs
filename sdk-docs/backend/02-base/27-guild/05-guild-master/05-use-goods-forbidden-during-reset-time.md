---
sidebar_label: "굿즈 사용"
description: "UseGoodsV5"
---

# UseGoodsV5

public BackendReturnObject **UseGoodsV5**(goodsType **type**, int **amount**);  
public BackendReturnObject **UseGoodsV5**(goodsType **type**, double **amount**);

:::info 안내
해당 함수는 랭킹 정산 에러를 방지하기 위해 리더보드 정산 시간을 제외한 시간에만 굿즈를 사용하는 함수입니다.  
기존 UseGoodsV3, UseGoodsV4 함수의 개선된 기능을 제공합니다.  
:::

## 파라미터

| Value  | Type             | Description                                     |
| ------ | ---------------- | ----------------------------------------------- |
| type   | goodsType(enum) | 기부할 재화의 종류. goodsType.goodsN(N : 1~10) |
| amount | int, double      | 사용할 금액의 양. 음수만 허용.                  |

## 설명

길드의 해당 goodsType의 재화를 사용합니다.  

리더보드에 초기화 주기가 설정 되어 있다면 리더보드 초기화 시간에는 변경이 불가능하며, 요청 시 428에러가 리턴됩니다.  

UseGoodsV3, UseGoodsV4와 사용법은 동일합니다.  

SDK 5.18.13부터 double 타입 amount를 사용할 수 있습니다.  
int 범위를 초과하는 큰 수치의 재화를 사용할 때 사용해 주세요. int 버전과 동일하게 음수 값을 전달하며, amount는 정수 값으로 처리되므로 소수점 이하 값은 버림 처리될 수 있습니다.  

## Example

### 동기

```js
Backend.Guild.UseGoodsV5(goodsType.goods1, -10);

// int 범위를 초과하는 값은 double 타입으로 전달할 수 있습니다.
Backend.Guild.UseGoodsV5(goodsType.goods1, -5000000000d);
```

### 비동기

```js
Backend.Guild.UseGoodsV5(goodsType.goods1, -10, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.UseGoodsV5, goodsType.goods1, -10, (callback) => {
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

**리더보드 정산 시간 사이에 호출한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required ranking is being counted
