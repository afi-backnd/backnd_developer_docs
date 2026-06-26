---
sidebar_label: returnValue 확인
---

# ReturnValue
public string **ReturnValue**;

## 설명
요청 성공 시 리턴된 RequestResult의 returnValue를 확인합니다.  
returnValue는 json 형태의 string입니다.  

## Example
```js
var reqResult = await BackndGuild.Instance.GetGuildsAsync();
if (reqResult.IsSuccess() == false)
{
    return;
}
var jsonString = reqResult.ReturnValue;
```

## Return Cases
**요청이 성공한 경우**  
json 문자열이 리턴됩니다.  

**요청이 실패한 경우**  
null
