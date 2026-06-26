---
sidebar_label: 트랜잭션 쓰기 V2
---

# TransactionWriteV2
public BackendReturnObject **TransactionWriteV2**(ListTransactionValue[\] **transactionList**);  
public BackendReturnObject **TransactionWriteV2**(List<TransactionValue\> **transactionList**);

## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| transactionList | [TransactionValue\[\] ](/sdk-docs/backend/base/game-information/transaction/crud/classes) | (selection) 트랜잭션 작업들을 배열 | 
| transactionList | List<TransactionValue\> | (selection) 트랜잭션 작업들을 저장한 리스트 | 

## 설명
transactionList에 존재하는 트랜잭션 명령들을 서버에 요청합니다.  
* 최대 10개의 작업을 1개로 묶을 수 있습니다.  
* **inDate**로 검색하는 TransactionValue의 경우, SetUpdate와 SetDelete 대신 **SetUpdateV2** 과 **SetDeleteV2**의 작업만 사용할 수 있습니다.  
* 작업 리스트에 존재하는 요청들 중 하나 이상의 작업에서 에러가 발생할 경우 모든 요청들은 실패 처리가 됩니다.  
* 모든 요청이 성공해야 모든 요청들이 성공 처리가 됩니다.  


## Example

### List< TransactionValue >
```js
List<TransactionValue> transactionList = new List<TransactionValue>();

// 삽입
Param param = new Param();
param.Add("name", "stat");
param.Add("level", 10);

transactionList.Add(TransactionValue.SetInsert("tableName", param));

//수정
Where where = new Where();
where.Equal("name", "stat");

Param updateParam = new Param();
updateParam.Add("atk", 20);

transactionList.Add(TransactionValue.SetUpdate("tableName", where, updateParam));


// 삭제
Where where2 = new Where();
where.Equal("name", "hero");

transactionList.Add(TransactionValue.SetDelete("tableName", where2));

//자신의 owner_inDate와 rowInDate로 데이터 수정/삭제
string data_inDate = "2021-09-16T01:23:13.232Z";
transactionList.Add(TransactionValue.SetUpdateV2("tableName2", data_inDate, Backend.UserInDate, updateParam));
transactionList.Add(TransactionValue.SetDeleteV2("tableName2", data_inDate, BackendUserInDate));

```

### 동기
```js
Backend.GameData.TransactionWriteV2(transactionList);  
```

### 비동기
```js
Backend.GameData.TransactionWriteV2(transactionList, (callback) => 
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.GameData.TransactionWriteV2, transactionList, (callback) => 
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**요청 중 Insert가 포함되어 있고, 모든 요청이 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : {"putItem":\[{"table":"삽입 성공한 테이블명","inDate":"삽입한 테이블의 inDate"}\]}
> 5개 테이블을 삽입한 경우의 예) : 
{ "putItem": \[
  {"table":"publicTest","inDate":"2020-06-22T03:54:44.659Z"},
  {"table":"publicTest","inDate":"2020-06-22T03:54:44.706Z"},
  {"table":"publicTest","inDate":"2020-06-22T03:54:44.709Z"},
  {"table":"privateTest","inDate":"2020-06-22T03:54:44.715Z"},
  {"table":"privateTest","inDate":"2020-06-22T03:54:44.717Z"} 
\] }

**모든 요청이 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**트랜잭션 작업을 수행 중 1개 이상의 작업에서 에러가 발생한 경우**  
에러 상황에 대응하는 에러가 리턴됩니다.  
Insert / Update / Delete를 수행 시 발생하는 에러는 각각의 개발자 문서를 참고해 주세요.  

**동일한 row에 2개 이상의 작업을 수행했을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Transaction request cannot include multiple operations on one item

**transactionList에 10개를 초과하는 작업이 있거나 혹은 작업이 존재하지 않는 경우**  
statusCode : 400  
errorCode : TransactionSizeError  
message : Not Support Transaction Size: {transactionList에 존재하는 작업 개수}

**transactionList에 Get 명령 이외의 작업이 존재하는 경우**  
statusCode : 400  
errorCode : TransactionDataError  
message : Not Support Type in TransactionRead: {에러가 발생한 명령}

**transactionList에 V2를 사용하지 않는 inDate를 이용한 수정/삭제 작업이 존재하는 경우**  
statusCode : 400  
errorCode : TransactionDataError  
message : V2 must use indate with owner_inDate: {에러가 발생한 명령}

**수정하려는 데이터의 크기가 4MB가 넘을 경우 / 수정하려는 컬럼의 갯수가 총 290개를 넘을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Invalid UpdateExpression: Expression size has exceeded the maximum allowed size;
