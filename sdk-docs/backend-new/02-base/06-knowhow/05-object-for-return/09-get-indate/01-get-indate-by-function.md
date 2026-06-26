---
sidebar_label: 함수를 이용하여 inDate 조회
---

# GetInDate
public string **GetInDate**();

## 설명
returnValue 내 존재하는 inDate 값을 확인합니다.  
inDate는 아래 조건을 기준으로 리턴됩니다.  
* returnValue에 inDate 키 값을 가진 데이터가 존재할 경우 해당 값이 리턴됩니다.  
* returnValue에 row 키 값을 가진 데이터가 존재하고, 해당 데이터 내에 inDate를 키 값으로 가진 데이터가 존재할 경우 해당 값이 리턴됩니다.  
* returnValue에 rows 키 값을 가진 데이터가 존재하고, 해당 리스트 내에 1번째 데이터(0번 인덱스)에 inDate를 키 값으로 가진 데이터가 존재할 경우 해당 값이 리턴됩니다. 

아래 조건에 해당 되는 경우에만 GetInDate 함수를 이용하여 inDate를 확인할 수 있습니다.  
* returnValue의 root 노드에 inDate, row, rows 키가 존재
* row 혹은 rows 값 내부의 inDate를 검색하는 경우 해당 값에 inDate 키가 존재
* inDate 키의 명이 "inDate"
> owner_inDate, gamerIndate 등 inDate의 키의 명이 inDate가 아닌 경우 GetInDate 함수로 확인할 수 없습니다.  

## Example
```js
var reqResult = await BackndUserData.Instance.CreateDataAsync("tableName");
string inDate = reqResult.GetInDate();
```

## Return Cases

**inDate 값이 존재하는 경우**  
inDate 값이 리턴됩니다.(ex. 2019-07-31T06:15:35.691Z)

**inDate 값이 존재하지 않는 경우**  
string.empty
