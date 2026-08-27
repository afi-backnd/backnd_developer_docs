---
sidebar_label: "데이터 검색 후 연산 (inDate이용)"
description: "UpdateWithCalculationV2"
---

# UpdateWithCalculationV2
public BackendReturnObject **UpdateWithCalculationV2**(string **tableName**, string **inDate**, string **owner_inDate**, Param **param**);

## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 업데이트할 테이블명 | 
| inDate   | string | 업데이트를 할 row의 inDate 값 | 
| owner_inDate   | string | 해당 row를 소유한 유저의 inDate | 
| param | [Param](/sdk-docs/backend/base/knowhow/param/param) | 업데이트할 정보(AddCalculation을 통해 데이터 삽입) |

## 설명
테이블에 저장되어 있는 값 중 inDate 컬럼의 값과 소유하는 유저의 owner_inDate가 일치하는 **row를 검색하여 사칙연산을 수행**합니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 수정할 수 있습니다.  
* 자기 자신의 public/private 데이터를 수정할 수 있습니다.  
* **타인의 public 데이터를 수정할 수 있습니다.**  
* **타인의 private 데이터를 수정할 수 없습니다.**  
> 자신의 owner_inDate는 { Backend.UserInDate }를 통해 가져올 수 있습니다.  

테이블에 저장되어 있는 number형 데이터를 사칙연산을 통해 수정합니다.  
기본 값에 입력한 숫자만큼 더하거나, 빼거나, 곱하거나, 나눌 수 있습니다.  
* 해당 함수를 사용할 때에는 인자의 Param이 반드시 AddCalculation()만을 사용해야 합니다.(Add()시 오류)
* 게임 정보 수정은 한 개의 Row에 대해서만 가능합니다.  
* where 쿼리로 여러 개의 row가 검색된 경우 제일 최근에 insert된 row가 수정됩니다.  
* 타인의 private 테이블은 수정할 수 없습니다.(public은 가능)
* Update()의 경우, 데이터를 수정하는 작업만 필요로 하지만, UpdateWithCalculation()는 데이터의 숫자 확인 및 수정 작업을 하기에 Update보다 DB를 더 많이 사용할 수 있습니다.  

### 예약된 컬럼

저장 시, **partition, gamer_id, inDate, updatedAt, sender, receiver, reservationDate, owner_inDate, client_date** 필드는 서버에서 사용하는 컬럼입니다.  
해당 컬럼이 param에 포함된 경우 에러가 발생합니다.  

| Value | Description | 뒤끝 콘솔 노출 여부 |  
| :----- | :----- | :----- |  
| partition | 서버에서 테이블을 구분하기 위해 사용하는 값 | X |  
| gamer_id | row 소유자의 게이머 ID | O |  
| inDate | row의 inDate (key 값) | O |  
| updatedAt | 테이블이 마지막으로 수정된 시각 | O |  
| sender | 뒤끝 기능을 위해 내부에서 사용하는 값 (우편 등) | X |  
| receiver | 뒤끝 기능을 위해 내부에서 사용하는 값 (우편 등) | X |  
| reservationDate | 뒤끝 기능을 위해 내부에서 사용하는 값 (우편 등) | X |  
| owner_inDate | row 소유자의 유저 inDate | O |  
| client_date | 처음으로 클라이언트에서 데이터 생성을 요청한 시간 | O |  

> owner_inDate 가 없는 구버전 테이블의 row의 경우 해당 함수를 이용하여 갱신하면 owner_inDate 컬럼이 추가됩니다.  

## Example

### 동기
```js
Param updateParam = new Param();
updateParam.AddCalculation("atk", GameInfoOperator.addition, 10); // 기존 데이터 atk에서 10만큼 더 추가
updateParam.AddCalculation("def", GameInfoOperator.subtraction, 20); // 기존 데이터 def에서 20만큼 감소

// 자신의 inDate의 row를 사칙연산
Backend.GameData.UpdateWithCalculationV2("tableName", inDate, Backend.UserInDate, updateParam);

//타인의 inDate의 row를 사칙연산
Backend.GameData.UpdateWithCalculationV2("tableName", inDate, "owner_inDate", updateParam);
```

### 비동기
```js
Param updateParam = new Param();
updateParam.AddCalculation("atk", GameInfoOperator.addition, 10); // 기존 데이터 atk에서 10만큼 더 추가
updateParam.AddCalculation("def", GameInfoOperator.subtraction, 20); // 기존 데이터 def에서 20만큼 감소

//자신의 inDate의 row를 사칙연산
Backend.GameData.UpdateWithCalculationV2("tableName", inDate, Backend.UserInDate, updateParam, callback =>
{
    // 이후 처리
});

//타인의 inDate의 row를 사칙연산
Backend.GameData.UpdateWithCalculationV2("tableName", inDate,  "owner_inDate", updateParam ,callback =>
{
    // 이후 처리
});
```

### SendQueue
```js
Param updateParam = new Param();
updateParam.AddCalculation("atk", GameInfoOperator.addition, 10); // 기존 데이터 atk에서 10만큼 더 추가
updateParam.AddCalculation("def", GameInfoOperator.subtraction, 20); // 기존 데이터 def에서 20만큼 감소

//자신의 inDate의 row를 사칙연산
SendQueue.Enqueue(Backend.GameData.UpdateWithCalculationV2, "tableName", inDate, Backend.UserInDate, updateParam ,callback =>
{
    // 이후 처리
});

//타인의 inDate의 row를 사칙연산
SendQueue.Enqueue(Backend.GameData.UpdateWithCalculationV2, "tableName", inDate, "owner_inDate", updateParam ,callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**수정에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**Param에 AddCalculation 형식으로 데이터를 수정하지 않은 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad operator have to been, 잘못된 operator have to been 입니다

**number형이 아닌 데이터의 수정을 시도한 경우**  
statusCode : 400  
errorCode : ValidationException  
message : An operand in the update expression has an incorrect data type

**존재하지 않는 컬럼에 수정을 시도한 경우**  
statusCode : 400  
errorCode : ValidationException  
message : An operand in the update expression has an incorrect data type

**(스키마) null이거나 존재하지 않는 컬럼에 수정을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : column not found, column을(를) 찾을 수 없습니다

**존재하지 않는 tableName인 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : table not found, table을(를) 찾을 수 없습니다

**존재하지 않는 inDate인 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gameInfo not found, gameInfo을(를) 찾을 수 없습니다

**partition, gamer_id, inDate, updatedAt, sender, receiver, reservationDate, owner_inDate 8가지 필드가 param에 포함된 경우**  
statusCode : 405  
errorCode : MethodNotAllowedParameterException  
message : MethodNotAllowed {param value}, 이용할 수 없는 {param value} 입니다

**비활성화된 table의 수정을 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  

**하나의 row(column들의 집합)이 400KB를 넘는 경우**  
statusCode : 413  
errorCode : ServerErrorException  
message : request entity too large
