---
sidebar_label: 트랜잭션 쓰기 (데이터 등록, 수정, 삭제)
description: "트랜잭션 쓰기 (데이터 등록, 수정, 삭제)"
---

# WriteTransaction
public Task&lt;WriteTransactionResult&gt; **WriteTransactionAsync**(TransactionWriteBuilder **builder**);

## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| builder   | TransactionWriteBuilder | 쓰기 전용 트랜잭션 명령 모음 클래스 | - |

### TransactionWriteBuilder
데이터 등록, 수정, 삭제를 한번에 요청할 수 있도록 명령을 추가하는 클래스입니다.

**데이터 등록**  
public TransactionWriteBuilder **AddCreateData**(string **tableName**, Param **param**);

```js
var writeBuilder = new TransactionWriteBuilder();

Param param = new Param();
param.Add("level", 10);
param.Add("hp", 20);

// 데이터 하나를 삽입
writeBuilder.AddCreateData("USER_DATA", param);

Param param2 = new Param();
param2.Add("score", 100000);

writeBuilder.AddCreateData("SCORE", param2);
```

**데이터 수정**  
public TransactionWriteBuilder **AddUpdateLatestData**(string **tableName**, Param **param**);  
public TransactionWriteBuilder **AddUpdateData**(string **tableName**, string **inDate**, Param **param**);  
public TransactionWriteBuilder **AddUpdateOtherData**(string **tableName**, string **inDate**, string **owner_inDate**, Param **param**);  

```js
var writeBuilder = new TransactionWriteBuilder();

Param param = new Param();
param.Add("level", 10);
param.Add("hp", 20);

writeBuilder.AddUpdateLatestData("USER_DATA", param);

Param param2 = new Param();
param2.Add("score", 10);

// 2023-10-30T08:34:49.925Z 는 SCORE 테이블에서 자신이 등록한 데이터의 row 값입니다.
writeBuilder.AddUpdateData("SCORE", "2023-10-30T08:34:49.925Z", param2);

Param param3 = new Param();
param3.Add("level", 10);
param3.Add("hp", 20);

// 2023-10-30T08:34:49.925Z 는 SCORE 테이블에서 "2022-09-10T04:54:12.314Z"와 owner_inDate가 일치하는 유저가 등록한 데이터의 row 값입니다.
// 2022-09-10T04:54:12.314Z 는 유저의 고유 ID 입니다.
writeBuilder.AddUpdateOtherData("SCORE", "2023-10-30T08:34:49.925Z", "2022-09-10T04:54:12.314Z", param3);
```
**데이터 삭제**  
public TransactionWriteBuilder **AddDeleteLatestData**(string **tableName**);  
public TransactionWriteBuilder **AddDeleteData**(string **tableName**, string **inDate**);  
public TransactionWriteBuilder **AddDeleteOtherData**(string **tableName**, string **inDate**, string **owner_inDate**);  
```js
var writeBuilder = new TransactionWriteBuilder();
writeBuilder.AddDeleteLatestData("USER_DATA");

// 2023-10-30T08:34:49.925Z 는 SCORE 테이블에서 자신이 등록한 데이터의 row 값입니다.
writeBuilder.AddDeleteData("SCORE", "2023-10-30T08:34:49.925Z");

// 2023-10-30T08:34:49.925Z 는 SCORE 테이블에서 "2022-09-10T04:54:12.314Z"와 owner_inDate가 일치하는 유저가 등록한 데이터의 row 값입니다.
// 2022-09-10T04:54:12.314Z 는 유저의 고유 ID 입니다.
writeBuilder.AddDeleteOtherData("SCORE", "2023-10-30T08:34:49.925Z", "2022-09-10T04:54:12.314Z");
```

## 설명
TransactionWriteBuilder에 존재하는 트랜잭션 명령들을 서버에 요청합니다.  
* TransactionWriteBuilder 내 명령 중 동일한 row 데이터를 수정, 삭제하는 명령이 존재할 경우, 에러가 발생합니다.  
* 최대 10개의 작업을 1개로 묶을 수 있습니다.  
* 작업 리스트에 존재하는 요청들 중 하나 이상의 작업에서 에러가 발생할 경우 모든 요청들은 실패 처리가 됩니다.  
* 모든 요청이 성공해야 모든 요청들이 성공 처리가 됩니다.  


## Example

### Task 형식
```js
var writeBuilder = new TransactionWriteBuilder();

Param param = new Param();
param.Add("level", 10);
param.Add("hp", 20);

// 데이터 하나를 삽입
writeBuilder.AddCreateData("USER_DATA", param);

Param param2 = new Param();
param2.Add("score", 10);
// 데이터 하나를 수정
writeBuilder.AddUpdateLatestData("SCORE", param2);

// 데이터 하나를 삭제
writeBuilder.AddDeleteLatestData("TEMP_DATA");

var reqResult = await BackndUserData.Instance.WriteTransactionAsync(writeBuilder);
```

### Callback 형식
```js
var writeBuilder = new TransactionWriteBuilder();

Param param = new Param();
param.Add("level", 10);
param.Add("hp", 20);

// 데이터 하나를 삽입
writeBuilder.AddCreateData("USER_DATA", param);

Param param2 = new Param();
param2.Add("score", 10);
// 데이터 하나를 수정
writeBuilder.AddUpdateLatestData("SCORE", param2);

// 데이터 하나를 삭제
writeBuilder.AddDeleteLatestData("TEMP_DATA");

BackndUserData.Instance.WriteTransaction(writeBuilder, callback => {
    // 이후 로직
});
```

## ReturnCase

### Success cases

**등록에 성공한 경우**    
statusCode : 200  
message : Success  
returnValue : 
> {"putItem":[{"table":"첫번째로 AddInsert한 테이블명","inDate":"생성된 데이터의 inDate값"},{"table":"두번째 AddInsert한 테이블명","inDate":"2023-10-30T09:00:52.209Z"},...],"ConsumedCapacity":[{"Write":{"CapacityUnits":18},"Read":{"CapacityUnits":0}}]}

**수정, 삭제에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue :
> {"putItem":[],"ConsumedCapacity":[{"Write":{"CapacityUnits":6},"Read":{"CapacityUnits":1}}]}

### Error cases

**트랜잭션 작업을 수행 중 1개 이상의 작업에서 에러가 발생한 경우**  
에러 상황에 대응하는 에러가 리턴됩니다.  
Create / Update / Delete를 수행 시 발생하는 에러는 각각의 개발자 문서를 참고해 주세요.  

**동일한 row에 2개 이상의 작업을 수행했을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Transaction request cannot include multiple operations on one item

**transactionList에 10개를 초과하는 작업이 있거나 혹은 작업이 존재하지 않는 경우**  
statusCode : 400  
errorCode : TransactionSizeError  
message : Not Support Transaction Size: {transactionList에 존재하는 작업 개수}

**수정하려는 데이터의 크기가 4MB가 넘을 경우 / 수정하려는 컬럼의 갯수가 총 290개를 넘을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Invalid UpdateExpression: Expression size has exceeded the maximum allowed size;
