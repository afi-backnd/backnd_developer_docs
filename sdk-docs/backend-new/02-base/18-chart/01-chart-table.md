---
sidebar_label: "게임 테이블 조회"
---

# GetTables
public Task< GetGameTablesResult > **GetTablesAsync()**;  
public Task< GetGameTablesResult > **GetTablesAsync(string **folderId**);  

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| folderId | string | 입력 시, 해당 폴더의 테이블만 불러옵니다. |


## 설명
콘솔에 등록한 테이블 목록을 조회합니다.  
* 파일을 선택하지 않은 테이블은 리스트에서 제외됩니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## GetGameTablesResult
```js
public class GameTableInfo
{
    public string Name;
    public string Id;
    public string Explain;
    public string SelectedFileId;
    public string CreatedDate;
}

public sealed class GetGameTablesResult : ListInfoResult<GameTableInfo>
{
    public List<GameTableInfo> GetInfoList();
}
```


## Example

### Task 방식
```js
var reqResult = await BackndGameTable.Instance.GetTablesAsync();
if (reqResult.IsSuccess() == false)
{
    Debug.LogError(reqResult);
    return;
}

foreach (var info in reqResult.GetInfoList())
{
    Debug.Log(info.Name);
    Debug.Log(info);
}
```

### Callback 방식
```js
BackndGameTable.Instance.GetTables(callback =>
{
    if (callback.IsSuccess() == false)
    {
        Debug.LogError(callback);
        return;
    }

    foreach (var info in callback.GetInfoList())
    {
        Debug.Log(info.Name);
        Debug.Log(info);
    }
});
```


## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

## ReturnValueJson
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
