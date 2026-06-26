---
sidebar_label: "[New] 굿즈 사용(리더 보드 초기화 시간 갱신 불가)"
---

# UseGoods

public Task< RequestResult > **UseGoodsAsync**(GoodsType **type**, int **amount**);

:::info 안내
해당 함수는 랭킹 정산 에러를 방지하기 위해 리더보드 정산 시간을 제외한 시간에만 굿즈를 사용하는 함수입니다.  
:::

## 파라미터

| Value  | Type             | Description                                     |
| ------ | ---------------- | ----------------------------------------------- |
| type   | GoodsType(enum)  | 기부할 재화의 종류. GoodsType.goodsN(N : 1~10) |
| amount | int              | 사용할 금액의 양. 음수만 허용.                  |

## 설명

길드의 해당 GoodsType의 재화를 사용합니다.  

리더보드에 초기화 주기가 설정 되어 있다면 리더보드 초기화 시간에는 변경이 불가능하며, 요청 시 428에러가 리턴됩니다.  


## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.UseGoodsAsync(GoodsType.goods1, -10);
```

### Callback 방식

```js
BackndGuild.Instance.UseGoods(goodsType.goods1, -10, (callback) =>
{
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
