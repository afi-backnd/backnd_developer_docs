---
sidebar_label: "차트 테이블 조회"
---

# Backend.CDN.Content.Table.Get
public BackendContentTableReturnObject **Backend.CDN.Content.Table.Get()**;  
public BackendContentTableReturnObject **Backend.CDN.Content.Table.Get**(string **folderId**);  

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| folderId | string | 입력 시, 해당 폴더의 차트 테이블만 불러옵니다. |


## 설명
콘솔에 등록한 차트 테이블 목록을 조회합니다.  
* 차트 파일을 선택하지 않은 차트는 리스트에서 제외됩니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## BackendContentTableReturnObject
```js
namespace BackEnd.Content
{
    public class ContentTableItem
    {
        public string chartName;
        public string chartId;
        public string chartExplain;
        public string selectedChartFileId;
        public string createdDate;
    }

    public class BackendContentTableReturnObject : BackendReturnObject
    {
        public List<ContentTableItem> GetContentTableItemList();
    }
}
```


## Example

### 동기
```js

BackEnd.Content.BackendContentTableReturnObject bro;

bro = Backend.CDN.Content.Table.Get();

if(bro.IsSuccess() == false) {
    Debug.LogError(bro);
    return;
}

foreach (BackEnd.Content.ContentTableItem item in bro.GetContentTableItemList())
{
    Debug.Log(item.chartName);
    Debug.Log(item);
}

```

### 비동기
```js
Backend.CDN.Content.Table.Get( bro =>
{
    if(bro.IsSuccess() == false) {
        Debug.LogError(bro);
        return;
    }

    foreach (BackEnd.Content.ContentTableItem item in bro.GetContentTableItemList())
    {
        Debug.Log(item.chartId);
        Debug.Log(item);
    }
});
```


## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON
``` js
{
  "rows": [
    {
      "chartName": {
        "S": "LevelChart"
      },
      "chartId": {
        "N": "23308"
      },
      "chartExplain": {
        "NULL": true
      },
      "selectedChartFileId": {
        "N": "120307"
      },
      "createdDate": {
        "S": "2024-05-07T03:26:17.000Z"
      }
    },
    {
      "chartName": {
        "S": "GoodsChart"
      },
      "chartId": {
        "N": "23310"
      },
      "chartExplain": {
        "NULL": true
      },
      "selectedChartFileId": {
        "N": "120309"
      },
      "createdDate": {
        "S": "2024-05-07T03:26:43.000Z"
      }
    }
  ]
}

```
