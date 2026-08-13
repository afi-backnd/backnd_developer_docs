---
sidebar_label: "언마샬된 Rows 확인"
description: "FlattenRows"
---

# FlattenRows
public LitJson.JsonData **FlattenRows**();

## 설명

요청 성공 시 리턴된 returnValue 내에 rows 키가 존재할 경우
rows의 값을 JsonData 객체로 변환한 후 모든 자료형을 제거하여 리턴하는 함수입니다.  

## Example
```js
var bro = Backend.GameData.Get("tableName", new Where(), 10);
if(bro.IsSuccess() == false) {
    return;
}

var json = bro.FlattenRows();
```

## Return Cases
**returnValue 에 rows 키가 존재하는 경우**  
언마샬 된 JsonData 객체가 리턴됩니다.  

**returnValue에 rows 키가 존재하지 않는 경우**  
빈 JsonData 객체가 리턴됩니다.  

**returnValue가 존재하지 않는 경우**  
빈 JsonData 객체가 리턴됩니다.  
