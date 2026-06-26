---
sidebar_label: "[Deprecated] DeleteLocalChartData"
description: "[Deprecated] DeleteLocalChartData"
---


# [Deprecated] DeleteLocalChartData
public void **DeleteLocalChartData**(string **chartKey**);

## 파라미터

| value  |  description |
| --- | --- |
| chartKey | **GetOneChartAndSave에서 지정한 차트 이름** 혹은 차트의 이름 혹은 차트의 ID. 어떤 Key 형태로 저장했느냐에 따라서 인자 값이 다르게 들어갈 수 있습니다.|

## 설명
로컬에 저장되어 있는 차트 파일을 삭제합니다.  

> 해당 함수는 void으로 반환되며, 동기 형식만 지원하고 있습니다.  

## Example

### 동기
```js
BackndLegacy.Chart.DeleteLocalChartData("chartKey");
```


## ReturnCase

### Error cases

**chartKey를 null 혹은 빈 문자열로 준 경우**  
**chartKey must not be null or empty** exception이 throw 됩니다.  

**존재하지 않는 chartKey를 삭제하고자 한 경우**  
**chartKey is not exist** exception이 throw 됩니다.  
