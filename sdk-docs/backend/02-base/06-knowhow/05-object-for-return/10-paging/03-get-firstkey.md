---
sidebar_label: "firstKey 확인"
description: "FirstKeyString"
---

# FirstKeyString
public string **FirstKeyString**();

## 설명
페이징 처리를 하기 위한 firstKey를 확인합니다.  

## Example
```js
var bro = Backend.GameData.Get("tableName", new Where(), 10);
if(bro.IsSuccess() == false) {
    return;
}

if(bro.HasFirstKey()) {
    var firstKey = bro.FirstKeystring();
}
```

## Return Cases
**firstKey가 존재하는 경우**  
firstKey가 리턴

**firstKey가 존재하지 않는 경우**  
string.Empty
