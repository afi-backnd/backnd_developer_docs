---
sidebar_label: "데이터 삭제(inDate이용)"
description: "DeleteV2"
---

# DeleteV2
public BackendReturnObject **Delete**(string **tableName**, string **inDate**, string **owner_inDate**);


## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 삭제할 게임 정보의  테이블명 | 
| inDate   | string | 삭제할 row의 inDate 값 | 
| owner_inDate   | string | 해당 row를 소유한 유저의 inDate | 

## 설명
테이블에 저장되어 있는 값 중 inDate 컬럼의 값과 소유하는 유저의 owner_inDate가 일치하는 **row를 검색하여 삭제**합니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 삭제할 수 있습니다.  
* 자기 자신의 public/private 데이터를 삭제할 수 있습니다.  
* **타인의 public 데이터를 삭제할 수 있습니다.**  
* **타인의 private 데이터를 삭제할 수 없습니다.**  
> 자신의 owner_inDate는 { Backend.UserInDate }를 통해 가져올 수 있습니다.  

## Example

### 동기
```js
// 자신의 inDate의 row를 제거
Backend.GameData.DeleteV2("tableName", inDate, Backend.UserInDate); 

//타인의 inDate의 row를 제거
Backend.GameData.DeleteV2("tableName", inDate, "owner_inDate"); 
```

### 비동기
```js
// 자신의 inDate의 row를 제거
Backend.GameData.DeleteV2("tableName", inDate, Backend.UserInDate, (callback) => 
{ 
    // 이후 처리
});

//타인 inDate의 row를 제거
Backend.GameData.DeleteV2("tableName", inDate, "owner_inDate", (callback) => 
{ 
    // 이후 처리
});
```

### SendQueue
```js
// 자신의 inDate의 row를 제거
SendQueue.Enqueue(Backend.GameData.DeleteV2, "tableName", inDate, Backend.UserInDate, (callback) => 
{ 
    // 이후 처리
});

// 타인 inDate의 row를 제거
SendQueue.Enqueue(Backend.GameData.DeleteV2, "tableName", inDate, "owner_inDate", (callback) => 
{ 
    // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**프라이빗 테이블의 타인 정보를 삭제하고자 하였을 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden Private table can only be modified by the owner, 금지된 Private table can only be modified by the owner

**존재하지 않는 row의 삭제를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gameInfo not found, gameInfo을(를) 찾을 수 없습니다

**존재하지 않는 table의 삭제를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : table not found, table을(를) 찾을 수 없습니다

**비활성화된 table의 삭제를 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다. 
