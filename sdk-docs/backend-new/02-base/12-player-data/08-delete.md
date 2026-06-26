---
sidebar_label: 유저 데이터 삭제하기
---

# DeleteData
public Task&lt;DeleteDataResult&gt; **DeleteLatestDataAsync**(string **tableName**, Param **param**)  
public Task&lt;DeleteDataResult&gt; **DeleteDataAsync**(string **tableName**, string **inDate**, Param **param**)  
public Task&lt;DeleteDataResult&gt; **DeleteOtherDataAsync**(string **tableName**, string **inDate**, string **owner_inDate**, Param **param**)  

## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| tableName   | string | 조회할 테이블명 | - |
| inDate   | string | 조회할 row의 inDate 값 | - |
| owner_inDate   | string | 해당 row를 소유한 유저의 inDate |  - |

## 설명
**DeleteLatestDataAsync** 함수는 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 하나를 삭제합니다.  
**DeleteDataAsync** 함수는 해당 테이블에 존재하는 자신의 데이터 중, inDate가 일치하는 데이터 하나를 삭제합니다.  
**DeleteOtherDataAsync** 함수는 해당 테이블에 존재하는 데이터 중, inDate와 owner_inDate가 일치하는 데이터 하나를 삭제합니다.(public일 경우, 타인의 데이터도 삭제할 수 있습니다.)  

* 스키마 정의/미정의 여부 관계없이 데이터를 삭제할 수 있습니다.  
* 자기 자신의 public/private 데이터를 삭제할 수 있습니다.  
* **타인의 public 데이터를 삭제할 수 있습니다.**  
* **타인의 private 데이터를 삭제할 수 없습니다.**  

## Example

### Task 형식
```js
// 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 수정
var reqResult1 = await BackndUserData.Instance.DeleteLatestDataAsync("tableName");

// 해당 테이블에 존재하는 자신의 데이터 중, inDate와 일치하는 데이터 수정
var reqResult2 = await BackndUserData.Instance.DeleteDataAsync("tableName", "2023-10-25:11:34:24.124Z");

// 해당 테이블에 존재하는 데이터 중, inDate, owner_inDate와 일치하는 데이터 수정(타인 데이터도 수정가능)
var reqResult3 = await BackndUserData.Instance.DeleteOtherDataAsync("tableName", "2023-10-25:11:34:24.124Z", "2023-10-22:09:14:35.616Z");
```

### Callback 형식
```js
// 해당 테이블에 존재하는 가장 최근에 등록된 내 데이터 삭제
BackndUserData.Instance.DeleteLatestData("tableName", (callback) =>
{
    // 이후 처리
});

// 해당 테이블에 존재하는 데이터 중, inDate와 일치하는 데이터 삭제
BackndUserData.Instance.DeleteData("tableName", "2023-10-25:11:34:24.124Z", (callback) =>
{
    // 이후 처리
});

// 해당 테이블에 존재하는 데이터 중, inDate, owner_inDate와 일치하는 데이터 삭제(타인 데이터도 삭제가능)
BackndUserData.Instance.DeleteOtherData("tableName", "2023-10-25:11:34:24.124Z", "2023-10-22:09:14:35.616Z", (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**삭제에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : {"ConsumedCapacity":{"Write":{"CapacityUnits":{읽기량}}}}
```예시 : {"ConsumedCapacity":{"Write":{"CapacityUnits":3}}}```

### Error cases

**존재하지 않는 유저의 owner_inDate 였을 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad owner_inDate, 잘못된 owner_inDate 입니다

**프라이빗 테이블에서 타인 정보를 삭제하고자 하였을 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad owner_inDate, 잘못된 owner_inDate 입니다

**존재하지 않는 table의 삭제를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : {테이블명} table not found, {테이블명} table을(를) 찾을 수 없습니다

**존재하지 않는 row의 삭제를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gameInfo not found, gameInfo을(를) 찾을 수 없습니다  

**비활성화된 table의 삭제를 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  
