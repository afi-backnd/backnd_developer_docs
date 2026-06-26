# SetInsert
public TransactionValue **SetInsert**(string **tableName**, Param **param**)

## 파라미터
| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 읽어올 테이블명 | 
| param   | [Param](/sdk-docs/backend/base/knowhow/param/Param) | 인서트 할 Param | 

## 설명
테이블을 삽입하는 명령을 리턴합니다.  

## Example
```js
List<TransactionValue> transactionValues = new List<TransactionValue>();

Param param = new Param();
param.Add("int", 100);
param.Add("string","문자열 아무거나");

transactionValues.Add(TransactionValue.SetInsert("tableName", param));
```
