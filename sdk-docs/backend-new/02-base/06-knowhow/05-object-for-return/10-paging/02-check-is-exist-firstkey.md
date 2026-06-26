---
sidebar_label: offsetKey 존재여부 확인
description: "offsetKey 존재여부 확인"
---

# HasOffsetKey
public bool **HasOffsetKey**();  

## 설명
returnValue에 offsetKey가 존재하는지 확인합니다.  

## Example
``` js
var reqResult = await BackndUserData.Instance.GetDataAsync("tableName", 10);
if (reqResult.HasOffsetKey())
{
    var offsetKey = reqResult.GetOffsetKey();
}
```

## Return Cases
**returnValue에 firstKey가 존재하는 경우**  
return : true  

**returnValue에 firstKey가 존재하지 않는 경우**  
return : false  

**returnValue가 존재하지 않는 경우**  
return : false  
