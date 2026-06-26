---
sidebar_label: returnValue 확인
---

# GetReturnValue
public string **GetReturnValue**();

## 설명
요청 성공 시 리턴된 BackendReturnObject의 returnValue를 확인합니다.  
returnValue는 json 형태의 string입니다.  

## Example
```js
var bro = Backend.Social.Guild.GetGuildListV3();
if(bro.IsSuccess() == false) {
    return;
}

var jsonString = bro.GetReturnValue();
```

## Return Cases
**요청이 성공한 경우**  
json 문자열이 리턴됩니다.  

**요청이 실패한 경우**  
null
