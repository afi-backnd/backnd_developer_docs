---
sidebar_label: 데이터 삭제(쿼리이용)
---

# Delete
public Task&lt;RequestResult&gt; **DeleteAsync**(string **tableName**, Where **where**);


## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| tableName   | string | 삭제할 게임 정보의  테이블명 | 
| where | [Where](/sdk-docs/backend/base/game-information/clause-where/basic)  | 검색할 where 절 |

## 설명
테이블에 저장되어 있는 값 중 where 조건문에 해당하는 **row 1개를 검색하여 삭제**합니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 삭제할 수 있습니다.  
* 삭제는 한 개의 row에 대해서만 가능합니다.  
* where 쿼리로 여러 개의 row가 검색된 경우 제일 최근에 생성된 row가 삭제됩니다.  
* **public/private 상관없이 자기 자신의 데이터만 삭제**할 수 있습니다.  

### 조건 없이 삭제를 시도한 경우
Where 조건을 new Where() 과 같이 조건을 주지 않은 경우 Update를 시도한 유저가 소유한 row 중 가장 최근에 삽입된 row가 삭제됩니다.  

## Example

### Task 형식
```js
// itemCode 컬럼이 dragon_sword 인 제일 최신 데이터 제거
Where where = new Where();
where.Equal("itemCode", "dragon_sword");
var reqResult = await BackndLegacy.GameData.DeleteAsync("tableName", where);
```

### Callback 형식
```js
// itemCode 컬럼이 dragon_sword 인 제일 최신 데이터 제거
Where where = new Where();
where.Equal("itemCode", "dragon_sword");
BackndLegacy.GameData.Delete("tableName", where, (callback) =>
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

**존재하지 않는 row의 삭제를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gameInfo not found, gameInfo을(를) 찾을 수 없습니다

**존재하지 않는 table의 삭제를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : table not found, table을(를) 찾을 수 없습니다

**where 조건으로 delete할 테이블을 검색했으나 테이블이 존재하지 않은 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gameInfo not found, gameInfo을(를) 찾을 수 없습니다

**비활성화된 table의 삭제를 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  

