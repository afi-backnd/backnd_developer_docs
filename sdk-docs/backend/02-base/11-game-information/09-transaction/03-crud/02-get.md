---
sidebar_label: SetGet, SetGetV2
---

# SetGet(Where), SetGetV2(inDate)  
public TransactionValue **SetGetV2**(string **tableName**, string **inDate**, string **owner_inDate**);  
public TransactionValue **SetGet**(string **tableName**, Where **where**);  

## 파라미터
| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 수정할 테이블명 | 
| inDate   | string | (selection) 검색할 row의 inDate | 
| owner_inDate   | string | (selection) 해당 row를  소유하고 있는 유저의 inDate | 
| where| [Where](/sdk-docs/backend/base/game-information/clause-where/basic)   | (selection) 검색할 row를 검색하기 위한 where 조건| 

## 설명
테이블을 조회하는 명령을 리턴합니다.  
> TransactionReadV2에서 inDate를 이용한 읽기 시, SetGetV2를 사용해야하며 SetGet는 사용하실 수 없습니다.  

## Example
```js
List<TransactionValue> transactionValues = new List<TransactionValue>();

//자신의 inDate의 row일 경우
transactionValues.Add(TransactionValue.SetGetV2("tableName", "row indate", Backend.UserInDate));
//xk inDate의 row일 경우
transactionValues.Add(TransactionValue.SetGetV2("tableName", "row indate","owner_inDate"));

// OR
Where where = new Where();
where.Equal("owner_inDate", "검색하려는 유저의 inDate");
transactionValues.Add(TransactionValue.SetGet("tableName", where));
```
