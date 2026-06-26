---
sidebar_label: returnValue 존재여부 확인
---

# GetJObject
public Newtonsoft.Json.Linq.JObject **GetJObject**();

## 설명
요청 성공 시 리턴된 returnValue를 JObject 로 변환하여 리턴합니다.  
> JObject는 Newtonsoft json 라이브러리에서 json 데이터를 다루기 위해 제공하는 클래스 입니다.  
GetJObject는 마셜링 데이터가 있는 경우 내부에서 자동으로 언마셜링을 하여 데이터를 재구성합니다.
JObject에 대한 자세한 설명은 [이곳](https://www.newtonsoft.com/json)을 확인해주세요.  

## Example
```js
var reqResult = await BackndAuth.Instance.GetUserInfoAsync();
if(reqResult.IsSuccess() == false) {
    return;
}

var json = reqResult.GetJObject();
```

## Return Cases
**요청이 성공한 경우**  
JObject 객체가 리턴됩니다.  

**요청이 실패한 경우**  
null
