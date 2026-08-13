---
sidebar_label: "returnValue 존재여부 확인"
description: "GetReturnValuetoJSON"
---

# GetReturnValuetoJSON
public LitJson.JsonData **GetReturnValuetoJSON**();

## 설명
요청 성공 시 리턴된 returnValue를 JsonData로 변환하여 리턴합니다.  
> JsonData는 LitJson 라이브러리에서 json 데이터를 다루기 위해 제공하는 클래스 입니다.  
JsonData에 대한 자세한 설명은 [이곳](https://litjson.net/api/LitJson/JsonData/)을 확인해주세요.  

## Example
```js
var bro = Backend.BMember.GetUserInfo();
if(bro.IsSuccess() == false) {
    return;
}

var json = bro.GetReturnValuetoJSON();
```

## Return Cases
**요청이 성공한 경우**  
JsonData 객체가 리턴됩니다.  

**요청이 실패한 경우**  
null
