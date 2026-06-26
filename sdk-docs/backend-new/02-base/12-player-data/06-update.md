---
sidebar_label: 유저 데이터 갱신하기
description: "유저 데이터 갱신하기"
---

# UpdateData
public Task&lt;UpdateDataResult&gt; **UpdateLatestDataAsync**(string **tableName**, Param **param**)  
public Task&lt;UpdateDataResult&gt; **UpdateDataAsync**(string **tableName**, string **inDate**, Param **param**)  
public Task&lt;UpdateDataResult&gt; **UpdateOtherDataAsync**(string **tableName**, string **inDate**, string **owner_inDate**, Param **param**)  

## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 업데이트를 할 테이블명 | 
| inDate   | string | 업데이트를 할 row의 inDate 값 | 
| owner_inDate   | string | 해당 row를 소유한 유저의 inDate | 
| param | [Param](/sdk-docs/backend/base/knowhow/param/Param) | 업데이트할 정보 |

## 설명
**UpdateLatestDataAsync** 함수는 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 하나를 수정합니다.  
**UpdateDataAsync** 함수는 해당 테이블에 존재하는 자신의 데이터 중, inDate가 일치하는 데이터 하나를 수정합니다.  
**UpdateOtherDataAsync** 함수는 해당 테이블에 존재하는 데이터 중, inDate와 owner_inDate가 일치하는 데이터 하나를 수정합니다.(public일 경우, 타인의 데이터도 수정할 수 있습니다.)  
* 스키마 정의/미정의 여부 관계없이 데이터를 수정할 수 있습니다.  
* 자기 자신의 public/private 데이터를 수정할 수 있습니다.  
* **타인의 public 데이터를 수정할 수 있습니다.**  
* **타인의 private 데이터를 수정할 수 없습니다.**  
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
* param에 존재하지 않는 컬럼이 수정되지 않습니다.  

param에 row 내 존재하지 않는 컬럼이 존재할 경우 아래와 같이 동작합니다.  
* 스키마 미정의 테이블의 경우 row에 존재하지 않는 컬럼을 update 한 경우 새로운 컬럼이 추가됩니다.  
* 스키마 정의 테이블의 경우 스키마를 선언하지 않은 컬럼을 update 한 경우 에러가 발생합니다.  

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
param.Add("gold", 100);
param.Add("level", 2);

// 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 수정
var reqResult1 = await BackndUserData.Instance.UpdateLatestDataAsync("tableName", param);

// 해당 테이블에 존재하는 자신의 데이터 중, inDate와 일치하는 데이터 수정
var reqResult2 = await BackndUserData.Instance.UpdateDataAsync("tableName", "2023-10-25:11:34:24.124Z", param);

// 해당 테이블에 존재하는 데이터 중, inDate, owner_inDate와 일치하는 데이터 수정(타인 데이터도 수정가능)
var reqResult3 = await BackndUserData.Instance.UpdateOtherDataAsync("tableName", "2023-10-25:11:34:24.124Z", "2023-10-22:09:14:35.616Z", param);
```

### Callback 형식
```js
Param param = new Param();
param.Add("gold", 100);
param.Add("level", 2);

// 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 수정
BackndUserData.Instance.UpdateLatestDataAsync("tableName", param, (callback) =>
{
    // 이후 처리
});

// 해당 테이블에 존재하는 데이터 중, inDate와 일치하는 데이터 수정
BackndUserData.Instance.UpdateDataAsync("tableName", "2023-10-25:11:34:24.124Z", param, (callback) =>
{
    // 이후 처리
});

// 해당 테이블에 존재하는 데이터 중, inDate, owner_inDate와 일치하는 데이터 수정(타인 데이터도 수정가능)
BackndUserData.Instance.UpdateOtherDataAsync("tableName", "2023-10-25:11:34:24.124Z", "2023-10-22:09:14:35.616Z", param, (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**수정에 성공한 경우**  
statusCode : 200
message : Success
returnValue : {"ConsumedCapacity":{"Read":{"CapacityUnits":6},"Write":{"CapacityUnits":4}}}

### Error cases

**(스키마) 스키마를 정의하지 않은 컬럼이 param에 포함된 경우**  
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
message : bad column data length, {설정한 list 컬럼}'s max length is {최대 크기}, 잘못된 column data length, {설정한 list 컬럼}'s max length is {최대 크기} 입니다
> bad column data length, listData's max length is 20, 잘못된 column data length, listData's max length is 20 입니다

**(스키마) 스키마에 map 컬럼을 선언할 때 선택한 map의 크기와 param에 입력한 map의 크기가 다른 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad column data length, {설정한 map 컬럼}'s max length is {최대 크기}, 잘못된 column data length, {설정한 map 컬럼}'s max length is {최대 크기} 입니다
> bad column data length, dicData's max length is 3, 잘못된 column data length, dicData's max length is 3 입니다

**연산하려는 column의 갯수가 총 290개를 넘을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Invalid UpdateExpression: Expression size has exceeded the maximum allowed size;

**수정하려는 컬럼의 갯수가 총 290개를 넘을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Invalid UpdateExpression: Expression size has exceeded the maximum allowed size;

**private 테이블에서 타인의 정보 수정하고자 하는 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden Private table can only be modified by the owner, 금지된 Private table can only be modified by the owner

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
message : MethodNotAllowed {param value}, 이용할 수 없는 {param value}입니다


**비활성화된 테이블의 수정을 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  

**업데이트할 데이터의 크기가 400KB를 넘는 경우**  
statusCode : 413  
errorCode : ServerErrorException  
message : request entity too large
