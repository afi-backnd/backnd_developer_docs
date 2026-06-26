---
sidebar_label: "[Deprecated] GetLocalChartData"
draft: true
unlisted: true
---

# [Deprecated] GetLocalChartData
public string **GetLocalChartData**(string **chartKey**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --- |
| chartKey |String | **GetOneChartAndSave에서 지정한 차트 이름** 혹은 차트의 이름 혹은 차트의 ID. 어떤 Key 형태로 저장했느냐에 따라서 인자 값이 다르게 들어갈 수 있습니다. |

## 설명
로컬에 저장된 차트 파일을 불러옵니다.  
로컬에 저장된 값은 뒤끝 콘솔의 차트 관리 항목에서 **업로드한 엑셀파일의 데이터**입니다.  

> 해당 함수는 string으로 반환되며, 동기 형식만 지원하고 있습니다.  

## Example

### 동기
```js
Backend.Chart.GetLocalChartData("chartKey");
```

## Return cases
**해당 chartKey가 로컬에 존재하는 경우**  
차트의 내용이 string 형태로 리턴됩니다.  
리턴되는 형태는 아래를 참고해 주세요.  

**해당 chartKey가 로컬에 존재하지 않는 경우**  
string.empty가 리턴됩니다.  

**chartKey를 null 혹은 빈 문자열로 준 경우**  
**chartKey must not be null or empty** exception이 throw 됩니다.  

### 로컬에 차트가 저장되는 형식
```js
{
    rows:
    [
        {
            column1:{
                S:"68a8b0f0-d336-11e7-8b06-4fc22765a737"
            },
            column2:{
                S:"123123"
            },
            num:{
                S:"1"
            }
        }
        .......  
    ]
}
```
### 불러온 value 사용 방법

저장된 값은 Json 형식의 String입니다. 이를 사용하기 위해 JsonObject로 변환해야 합니다.  
뒤끝은 LitJson을 사용하여 구현되어 있으나 다른 것을 사용해도 무방합니다.  

```js
LitJson.JsonData chartJson = JsonMapper.ToObject(Backend.Chart.GetLocalChartData(ChartName));

var rows = chartJson["rows"];

// 차트의 길이
Debug.Log(rows.Count);

// 차트의 10번째 행의 column1 내용을 읽어오기
Debug.Log(rows[10]["column1"]["S"].ToString());
```
