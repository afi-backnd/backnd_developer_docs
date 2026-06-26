---
sidebar_label: 데이터 수정 (쿼리이용)
---

# Update
public BackendReturnObject **Update**(string **tableName**, Where **where**, Param **param**);

## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 업데이트를 할 테이블명 | 
| where   | [Where](/sdk-docs/backend/base/game-information/clause-where/basic) | 업데이트를 할 row를 찾기 위한 param | 
| param | [Param](/sdk-docs/backend/base/knowhow/param/Param) | 업데이트할 정보 |

## 설명
테이블에 저장되어 있는 값 중 where 조건문에 해당하는 **row 1개를 검색하여 수정**합니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 수정할 수 있습니다.  
* 수정은 한 개의 row에 대해서만 가능합니다.  
* where 쿼리로 여러 개의 row가 검색된 경우 제일 최근에 삽입된 row가 수정됩니다.  
* **public/private 상관없이 자기 자신의 데이터만 수정할 수 있습니다.**  
* 숫자형 데이터의 경우, 최대 **9007199254740991(약 9000조)**까지 안전하게 값을 넣을 수 있습니다.  
그 이상의 데이터를 삽입할 경우, 일의 자리, 십의 자리 수는 0으로 내림처리가 됩니다.  
숫자가 증가하여 자리 수가 많아질수록 작은 수는 내림처리가 되며 큰수는 왼쪽 기준으로 16~18자리의 수까지 값이 유지됩니다.  
> long의 경우 다음과 같이 데이터가 저장됩니다.  

12345678912345678 -> 12345678912345678(9000조 이하, 기본값 유지)  
123456789123456789 -> 123456789123456780(9000조 이상, 내림 처리 발생)  
1234567891234567891 -> 1234567891234568000  
9223372036854775807 -> 9223372036854776000(long의 최대값)  

> 더 큰 데이터 저장을 위해 double로 정수를 저장할 경우 다음과 같이 표시됩니다.  

12345678912345678912 -> 12345678912345678000(long의 최대값 보다 더 큰 double)  
123456789123456789123.0 -> 123456789123456800000  
1234567891234567891234.0 -> 123456789123456800000000  
1.23456789123457E+29 -> 123456789123457000000000000000  

해당 수를 클라이언트(C#)에서 받을 경우 지수형태로 변경이 될 수 있습니다.  
12345678912345678000 -> 1.23456789123457E+19

param에 row 내 일부 컬럼만 존재할 경우 해당 컬럼만 수정됩니다.  
* param에 존재하지 않는 컬럼이 삭제되지 않습니다.  

param에 row 내 존재하지 않는 컬럼이 존재할 경우 아래와 같이 동작합니다.  
* 스키마 미정의 테이블의 경우 row에 존재하지 않는 컬럼을 update 한 경우 새로운 컬럼이 추가됩니다.  
* 스키마 정의 테이블의 경우 스키마를 선언하지 않은 컬럼을 update 한 경우 에러가 발생합니다.  

### 조건 없이 수정을 시도한 경우
Where 조건을 new Where() 과 같이 조건을 주지 않은 경우 Update를 시도한 유저가 소유한 row 중 가장 최근에 삽입된 row가 수정됩니다.  

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
// atk 컬럼의 값을 110으로 수정
Param param = new Param();
param.Add("atk", 110);

// key 컬럼의 값이 keyCode인 데이터 검색
Where where = new Where();
where.Equal("key", "keyCode");

Backend.GameData.Update("tableName", where, param);
```

### 비동기
```js
// atk 컬럼의 값을 110으로 수정
Param param = new Param();
param.Add("atk", 110);

// key 컬럼의 값이 keyCode인 데이터 검색
Where where = new Where();
where.Equal("key", "keyCode");

Backend.GameData.Update("tableName", where, param, (callback) => 
{
    // 이후 처리
});
```

### SendQueue
```js
// atk 컬럼의 값을 110으로 수정
Param param = new Param();
param.Add("atk", 110);

// key 컬럼의 값이 keyCode인 데이터 검색
Where where = new Where();
where.Equal("key", "keyCode");

SendQueue.Enqueue(Backend.GameData.Update, "tableName", where, param, (callback) => 
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

**(스키마) 스키마를 정의하지 않은 컬럼을 수정하려 한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad 컬럼이 존재하지 않습니다., 잘못된 컬럼이 존재하지 않습니다.  

**(스키마) 스키마를 정의할 때 선언한 컬럼의 데이터 타입과 update 하려는 컬럼의 데이터 타입이 다른 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad {column 명} dataType, 잘못된 {column 명} dataType 입니다

**(스키마) 스키마에 list 컬럼을 선언할 때 선택한 list의 크기와 param에 입력한 list의 크기가 다른 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad list data length, 잘못된 list data length 입니다

**(스키마) 스키마에 map 컬럼을 선언할 때 선택한 map의 크기와 param에 입력한 map의 크기가 다른 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad map data length, 잘못된 map data length 입니다

**수정하려는 컬럼의 갯수가 총 290개를 넘을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Invalid UpdateExpression: Expression size has exceeded the maximum allowed size;

**존재하지 않는 tableName인 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : table not found, table을(를) 찾을 수 없습니다

**where 조건으로 update할 테이블을 검색했으나 테이블이 존재하지 않은 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gameInfo not found, gameInfo을(를) 찾을 수 없습니다

**(스키마) 스키마를 정의하지 않은 컬럼을 where 조건으로 검색하려 한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gameInfo not found, gameInfo을(를) 찾을 수 없습니다

**partition, gamer_id, inDate, updatedAt, sender, receiver, reservationDate, owner_inDate 8가지 필드가 param에 포함된 경우**  
statusCode : 405  
errorCode : MethodNotAllowedParameterException  
message : MethodNotAllowed {param value}, 이용할 수 없는 {param value}입니다

**비활성화된 테이블의 수정을 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  

**업데이트할 데이터의 크기가 400KB를 넘는 경우**  
statusCode : 413  
errorCode : ServerErrorException  
message : request entity too large

