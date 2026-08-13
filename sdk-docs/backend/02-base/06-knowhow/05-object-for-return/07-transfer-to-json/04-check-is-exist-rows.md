---
sidebar_label: "Rows 존재여부 확인"
description: "HasRows"
---

# HasRows
public bool **HasRows**();  
public bool **HasRows**(out LitJson.JsonData **jsonData**, bool **isFlatten**);

## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| jsonData| jsonData| (optional) returnValue 내 rows키가 존재하는 경우 전달된 인수에 JsonData 형태의 rows 값을 할당합니다. | - |
| isFlatten | bool | (optional) jsonData에 존재하는 [자료형](/sdk-docs/backend/base/knowhow/object-for-return/data-structure) 제거 여부 | false |

## 설명

서버에 요청 결과 리턴된 BackendReturnObject 객체에 returnValue가 존재하는 경우 returnValue에 rows 키가 존재하는지 여부를 확인합니다.  

## Example
```js
var bro = Backend.GameData.Get("tableName", new Where(), 10);

LitJson.JsonData json = null;
if(bro.HasRows()) {
    json = bro.Rows();
}

if(bro.HasRows(out json)) {
    // 이후 처리
}
```

## Return Cases
**returnValue 내 rows 키가 존재하고 isFlatten이 false인 경우**  
return : true
jsonData: [Rows](/sdk-docs/backend/base/knowhow/object-for-return/transfer-to-json/check-rows)

**returnValue 내 rows 키가 존재하고 isFlatten이 true인 경우**  
return : true
jsonData : [FlattenRows](/sdk-docs/backend/base/knowhow/object-for-return/transfer-to-json/check-unmarshalled-rows)

**returnValue 내 rows 키가 존재하지 않는 경우**  
return : false
jsonData : 빈 JsonData 객체가 리턴됩니다.  

**returnValue가 존재하지 않는 경우**  
return : false
jsonData : 빈 JsonData 객체가 리턴됩니다.  
