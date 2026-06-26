---
sidebar_label: "[unlisted] returnValue 확인"
draft: true
unlisted: true
---

# HasReturnValue
public bool **HasReturnValue**();  
public bool **HasReturnValue**(out LitJson.JsonData **jsonData**, bool **isFlatten**);

## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| jsonData| jsonData| (optional) returnValue가 존재하는 경우 전달된 인수에 JsonData 형태의 returnValue를 할당합니다. | - |
| isFlatten | bool | (optional) returnValue에 존재하는 [자료형](/sdk-docs/backend/base/knowhow/object-for-return/data-structure) 제거 여부 | false |

## 설명

서버에 요청 결과 리턴된 RequestResult 객체에 returnValue가 존재하는지 확인합니다.  
returnValue는 요청이 성공한 경우에만 존재합니다.  

## Example
```js
var bro = Backend.BMember.GetUserInfo();

LitJson.JsonData jsonData = null;
if(bro.HasReturnValue()) {
    jsonData = bro.GetReturnValuetoJSON();
}

if(bro.HasReturnValue(out jsonData)) {
    // 이후 처리
}
```

## Return Cases
**returnValue가 존재하고 isFlatten이 false인 경우**  
return : true
jsonData : [GetReturnValuetoJSON](/sdk-docs/backend/base/knowhow/object-for-return/transfer-to-json/check-returned-value)

**returnValue가 존재하고 isFlatten이 true인 경우**  
return : true
jsonData : [GetFlattenJSON](/sdk-docs/backend/base/knowhow/object-for-return/transfer-to-json/check-unmarshalled-rows)

**returnValue가 존재하지 않는 경우**  
return : false
jsonData : null
