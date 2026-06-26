---
sidebar_label: offsetKey 가져오기
description: "offsetKey 가져오기"
---

# GetOffsetKey
public string **GetOffsetKey**();

## 설명
페이징 처리를 하기 위한 offsetKey를 가져옵니다.

## Example
```js
var reqResult = await BackndUserData.Instance.GetDataAsync("tableName", 10);
if (reqResult.IsSuccess() == false)
{
    return;
}

if (reqResult.HasOffsetKey())
{
    var offsetKey = reqResult.GetOffsetKey();
}
```

## Return Cases
**offsetKey가 존재하는 경우**  
offsetKey가 리턴

**offsetKey가 존재하지 않는 경우**  
string.Empty
