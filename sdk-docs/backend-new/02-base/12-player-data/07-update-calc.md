---
sidebar_label: 유저 데이터 연산하기
description: "유저 데이터 연산하기"
---

# UpdateDataWithCalculation
public Task&lt;UpdateDataResult&gt; **UpdateLatestDataWithCalculationAsync**(string **tableName**, Param **param**)  
public Task&lt;UpdateDataResult&gt; **UpdateDataWithCalculationAsync**(string **tableName**, string **inDate**, Param **param**)  
public Task&lt;UpdateDataResult&gt; **UpdateOtherDataWithCalculationAsync**(string **tableName**, string **inDate**, string **owner_inDate**, Param **param**)  

## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 업데이트할 테이블명 | 
| inDate   | string | 업데이트를 할 row의 inDate 값 | 
| owner_inDate   | string | 해당 row를 소유한 유저의 inDate | 
| param | [Param](/sdk-docs/backend/base/knowhow/param/Param) | 업데이트할 정보(AddCalculation을 통해 데이터 삽입) |


## 설명
**UpdateLatestDataWithCalculationAsync** 함수는 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 하나를 덧셈 혹은 뺄셈을 통해 수정합니다.  
**UpdateDataWithCalculationAsync** 함수는 해당 테이블에 존재하는 자신의 데이터 중, inDate가 일치하는 데이터 하나를 덧셈 혹은 뺄셈을 통해 수정합니다.  
**UpdateOtherDataWithCalculationAsync** 함수는 해당 테이블에 존재하는 데이터 중, inDate와 owner_inDate가 일치하는 데이터 하나를 덧셈 혹은 뺄셈을 통해 수정합니다.(public일 경우, 타인의 데이터도 수정할 수 있습니다.)  
* 스키마 정의/미정의 여부 관계없이 데이터를 수정할 수 있습니다.  
* 자기 자신의 public/private 데이터를 수정할 수 있습니다.  
* **타인의 public 데이터를 수정할 수 있습니다.**  
* **타인의 private 데이터를 수정할 수 없습니다.**  

테이블에 저장되어 있는 number형 데이터를 더하기 혹은 빼기를 통해 수정합니다.  
기본 값에 입력한 숫자만큼 더하거나, 뺄 수 있습니다.  
* 해당 함수를 사용할 때에는 인자의 Param이 반드시 AddCalculation()만을 사용해야 합니다.(Add()시 오류)
* 게임 정보 수정은 한 개의 Row에 대해서만 가능합니다.  
* 타인의 private 테이블은 수정할 수 없습니다.(public은 가능)
* UpdateData()의 경우, 데이터를 수정하는 작업만 필요로 하지만, UpdateWithCalculation()는 데이터의 숫자 확인 및 수정 작업을 하기에 Update보다 DB를 더 많이 사용할 수 있습니다.  

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

### Task 형식
```js
Param param = new Param();
param.AddCalculation("atk", GameInfoOperator.addition, 10); // 기존 데이터 atk에서 10만큼 더 추가
param.AddCalculation("def", GameInfoOperator.subtraction, 20); // 기존 데이터 def에서 20만큼 감소

// 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 수정
var reqResult1 = await BackndUserData.Instance.UpdateLatestDataWithCalculationAsync("tableName", param);

// 해당 테이블에 존재하는 자신의 데이터 중, inDate와 일치하는 데이터 수정
var reqResult2 = await BackndUserData.Instance.UpdateDataWithCalculationAsync("tableName", "2023-10-25:11:34:24.124Z", param);

// 해당 테이블에 존재하는 데이터 중, inDate, owner_inDate와 일치하는 데이터 수정(타인 데이터도 수정가능)
var reqResult3 = await BackndUserData.Instance.UpdateOtherDataWithCalculationAsync("tableName", "2023-10-25:11:34:24.124Z", "2023-10-22:09:14:35.616Z", param);
```

### Callback 형식
```js
Param param = new Param();
param.AddCalculation("atk", GameInfoOperator.addition, 10); // 기존 데이터 atk에서 10만큼 더 추가
param.AddCalculation("def", GameInfoOperator.subtraction, 20); // 기존 데이터 def에서 20만큼 감소

// 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 수정
BackndUserData.Instance.UpdateLatestDataWithCalculation("tableName", param, (callback) =>
{
    // 이후 처리
});

// 해당 테이블에 존재하는 데이터 중, inDate와 일치하는 데이터 수정
BackndUserData.Instance.UpdateDataWithCalculation("tableName", "2023-10-25:11:34:24.124Z", param, (callback) =>
{
    // 이후 처리
});

// 해당 테이블에 존재하는 데이터 중, inDate, owner_inDate와 일치하는 데이터 수정(타인 데이터도 수정가능)
BackndUserData.Instance.UpdateOtherDataWithCalculation("tableName", "2023-10-25:11:34:24.124Z", "2023-10-22:09:14:35.616Z", param, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**수정에 성공한 경우**  
statusCode : 200
message : Success
returnValue : {"ConsumedCapacity":{"Read":{"CapacityUnits":1},"Write":{"CapacityUnits":4}}}

### Error cases

**Param.AddCalculation()으로 GameInfoOperator.multiplication(곱셈)과 GameInfoOperator.division(나눗셈)이 들어간 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Can't use multiplication and division in param.AddCalculation(GameOperator)


**(스키마) 스키마를 정의할 때 선언한 컬럼의 데이터 타입과 InsertData 하려는 데이터 타입이 다른 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad {column 명} dataType, 잘못된 {column 명} dataType 입니다

**(스키마) 스키마를 정의하지 않은 컬럼을 InsertData 하려고 시도한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad 컬럼이 존재하지 않습니다., 잘못된 컬럼이 존재하지 않습니다.  

**(스키마) 스키마에 list 컬럼을 선언할 때 설정한 list의 크기와 param에 입력한 list의 크기가 더 클 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad list data length, 잘못된 list data length 입니다

**(스키마) 스키마에 map 컬럼을 선언할 때 선택한 map의 크기와 param에 입력한 map의 크기가 다를 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad map data length, 잘못된 map data length 입니다

**등록하려는 컬럼의 갯수가 총 290개를 넘을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Invalid UpdateExpression: Expression size has exceeded the maximum allowed size;

**등록 시도한 데이터의 크기가 400KB를 넘는 경우**  
statusCode : 400
errorCode : ValidationException
message : Item size has exceeded the maximum allowed size

**등록 시도한 데이터의 크기가 400KB를 넘는 경우**  
statusCode : 400
errorCode : ValidationException
message : Item size to update has exceeded the maximum allowed size

**존재하지 않는 table에 InsertData를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : table not found, table을(를) 찾을 수 없습니다

**partition, gamer_id, inDate, updatedAt, sender, receiver, reservationDate, owner_inDate 8가지 필드가 param에 포함된 경우**  
statusCode : 405  
errorCode : MethodNotAllowedParameterException  
message : MethodNotAllowed {param value}, 이용할 수 없는 {param value}입니다

**비활성화된 tableName에 등록을 시도한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  

**등록 시도한 데이터의 크기가 400KB를 넘는 경우**  
statusCode : 413  
errorCode : ServerErrorException  
message : request entity too large

