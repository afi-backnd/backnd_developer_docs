---
sidebar_label: 리턴값내 inDate를 직접조회
description: "리턴값내 inDate를 직접조회"
---

# 리턴 값 내 inDate를 직접 조회하기
아래 예제를 참고하면
GetObject(), GetRows(), 함수를 이용하여 리턴된 json 객체 내에서 inDate값을 조회할 수 있습니다.  

아래 예제는 리턴된 json 객체 내 0번 인덱스의 inDate 값을 조회하는 예제입니다.  
## Example
```js
var reqResult = await BackndUserData.Instance.GetDataAsync("tableName");

// GetJObject()
var jsons = reqResult.GetJObject();
var inDate = jsons["rows"][0]["inDate"].ToString();

// GetRows()
var rows = reqResult.GetRows();
inDate = rows[0]["inDate"].ToString();
```
