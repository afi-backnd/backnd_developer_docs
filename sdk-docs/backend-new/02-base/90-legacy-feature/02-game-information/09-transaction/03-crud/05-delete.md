---
sidebar_label: SetDelete, SetDeleteV2
description: "SetDelete, SetDeleteV2"
---

# SetDelete(Where), SetDeleteV2(inDate)  
public TransactionValue **SetDeleteV2**(string **tableName**, string **indate**, string **owner_inDate**)  
public TransactionValue **SetDelete**(string **tableName**, Where **where**)


## 파라미터
| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 수정할 테이블명 | 
| inDate   | string | (selection) 삭제할 row의 inDate | 
| owner_inDate   | string | 해당 row를 가지고 있는 유저의 inDate | 
| where| [Where](/sdk-docs/backend/base/game-information/clause-where/basic)   | (selection) 삭제할 row를 검색하기 위한 where 조건| 

## 설명
테이블을 삭제하는 명령을 리턴합니다.  
> TransactionWriteV2에서 inDate를 이용한 삭제 시, SetDeleteV2를 사용해야하며 SetDelete는 사용하실 수 없습니다.  

## Example
```js
List<TransactionValue> transactionValues = new List<TransactionValue>();

//자신의 inDate의 row를 삭제 시
transactionValues.Add(TransactionValue.SetDeleteV2("tableName", "indate", BackndBase.UserInDate));

//자신의 inDate의 row를 삭제 시
transactionValues.Add(TransactionValue.SetDeleteV2("tableName", "indate", "owner_inDate"));

// OR

Where where = new Where();
where.Equal("name", "dragon_sword");
transactionValues.Add(TransactionValue.SetDelete("tableName", where));
```
