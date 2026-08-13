---
sidebar_label: "SetUpdate, SetUpdateV2"
description: "SetUpdate(Where), SetUpdateV2(inDate)"
---

# SetUpdate(Where), SetUpdateV2(inDate)  
public TransactionValue **SetUpdateV2**(string **tableName**, string **inDate**, string **owner_inDate**, Param **param**)  
public TransactionValue **SetUpdate**(string **tableName**, Where **where**, Param **param**)

## 파라미터
| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 수정할 테이블 명 | 
| inDate   | string | (selection)수정할 row의 inDate | 
| owner_inDate   | string | (selection)해당 row를 가지고 있는 유저의 inDate | 
| where| [Where](/sdk-docs/backend/base/game-information/clause-where/basic)   | (selection)수정할 row를 검색하기 위한 where 조건| 
| param | [Param](/sdk-docs/backend/base/knowhow/param/Param) | 수정할 정보 |

## 설명
테이블을 수정하는 명령을 리턴합니다.  
> TransactionWriteV2에서 inDate를 이용한 수정 시, SetUpdateV2를 사용해야하며 SetUpdate는 사용하실 수 없습니다.  

## Example
```js
Param param = new Param();
param.Add("data", 777);

List<TransactionValue> transactionValues = new List<TransactionValue>();

//자신의 row의 inDate 수정 시
transactionValues.Add(TransactionValue.SetUpdateV2("tableName", "inDate", Backend.UserInDate, param));
//타인의 row의 inDate 수정 시
transactionValues.Add(TransactionValue.SetUpdateV2("tableName", "inDate", "owner_inDate", param));

// OR
Where where = new Where();
where.Equal("name", "dragon_sword");
transactionValues.Add(TransactionValue.SetUpdate("tableName", where, param));
```
