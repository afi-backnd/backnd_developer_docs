---
sidebar_label: returnValue 존재여부 확인
---

# HasReturnValue
public bool **HasReturnValue**();  
public bool **HasReturnValue**(out string **jsonString**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| jsonString | string | (optional) returnValue가 존재하는 경우 전달된 인수에 returnValue를 할당합니다. |

## 설명

서버에 요청 결과 리턴된 BackendReturnObject 객체에 returnValue가 존재하는지 확인합니다.  
returnValue는 요청이 성공한 경우에만 존재합니다.  

## Example
```js
var bro = Backend.BMember.GetUserInfo();

string jsonString = null;
if(bro.HasReturnValue()) {
    jsonString = bro.GetReturnValue();
    Debug.Log(jsonString);
}

if(bro.HasReturnValue(out jsonString)) {
    Debug.Log(jsonString);
}
```

## Return Cases
**returnValue가 존재하는 경우**  
return : true
jsonString : returnValue

**returnValue가 존재하지 않는 경우**  
return : false
jsonString : null
