---
sidebar_label: Rows 존재여부 확인
description: "Rows 존재여부 확인"
---

# HasRows
public bool **HasRows**();  
public bool **HasRows**(string **rowsKey**);

## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| rowsKey | string | (optional) returnValue 내 해당 rows를 체크하기 위한 key값 | - |

## 설명

서버에 요청 결과 리턴된 RequestResult 객체에 returnValue가 존재하는 경우 returnValue에 rows 키가 존재하는지 여부를 확인합니다.  
rowsKey값을 넣지 않으면 기본값으로 "rows" 값에 대해 체크합니다.  

## Example
```js
var reqResult = await BackndUserData.Instance.GetDataAsync("tableName", 10);        
if (reqResult.HasRows())
{
    var rows = reqResult.GetRows();
}

if (reqResult.HasRows("rows"))
{
    // 이후 처리
}
```

## Return Cases
**returnValue 내 rows 키가 존재하지 않는 경우**  
return : false

**returnValue가 존재하지 않는 경우**  
return : false
