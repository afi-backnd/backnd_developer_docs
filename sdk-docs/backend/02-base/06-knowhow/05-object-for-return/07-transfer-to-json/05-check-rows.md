---
sidebar_label: "Rows 확인"
description: "Rows"
---

# Rows
public LitJson.JsonData **Rows**();

## 설명
요청 성공 시 리턴된 returnValue 내에 rows 키가 존재할 경우
rows의 값만 JsonData 형식으로 리턴하는 함수입니다.  

### rows 키
rows 키는 returnValue에 데이터가 리스트 형태로 리턴 될 때 
대부분의 함수에서 리스트의 상위 노드로 사용하는 키 입니다.  

## Example
```js
var bro = Backend.GameData.Get("tableName", new Where(), 10);
if(bro.IsSuccess() == false) {
    return;
}

var rows = bro.Rows();
```

## Return Cases
**요청이 성공하고, returnValue에 rows 키가 존재하는 경우**  
rows의 값이 JsonData 객체가 리턴됩니다.  

**요청이 성공하고, returnValue에 rows 키가 존재하지 않는 경우**  
빈 JsonData 객체가 리턴됩니다.  

**요청이 실패한 경우**  
빈 JsonData 객체가 리턴됩니다.  
