---
sidebar_label: "언마샬 된 returnValue 확인"
description: "GetFlattenJson"
---

# GetFlattenJson
public LitJson.JsonData **GetFlattenJson**();

## 설명
요청 성공 시 리턴 된 returnValue를 JsonData 객체로 변환 후
객체 내 존재하는 모든 자료형을 제거하여 리턴합니다.  

## Example
```js
var bro = Backend.BMember.GetUserInfo();
if(bro.IsSuccess() == false) {
    return;
}

var json = bro.GetFlattenJson();
```

## Return Cases
**returnValue가 존재하는 경우**  
언마샬 된 JsonData 객체가 리턴됩니다.  
