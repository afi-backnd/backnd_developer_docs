---
sidebar_label: "[Deprecated] 굿즈 기부"
draft: true
unlisted: true
---

# ContributeGoodsV3

public BackendReturnObject **ContributeGoodsV3**(goodsType **type**, int **amount**);

:::danger 랭킹 정산 도중 함수 호출 시 랭킹 정산 실패 문제
굿즈 정보를 이용한 길드 랭킹을 이용 시, **랭킹 정산 시간에 해당 함수를 호출할 경우 랭킹 정산이 비정상적으로 종료**될 수 있습니다.  

따라서 굿즈 정보를 이용한 길드 랭킹을 이용하는 경우, 4~5시에 호출에는 갱신이 불가능한  **ContributeGoodsV4** 기능을 이용해주시기 바랍니다.  
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

## Example

### 동기

```js
Backend.Guild.ContributeGoodsV3(goodsType.goods1, 20);
```

### 비동기

```js
Backend.Guild.ContributeGoodsV3(goodsType.goods1, 10, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.ContributeGoodsV3, goodsType.goods1, 10, (callback) => {
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

**길드에 속하지 않은 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : notGuildMember 사전 조건을 만족하지 않습니다.  
