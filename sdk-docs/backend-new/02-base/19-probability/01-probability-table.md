---
sidebar_label: "확률 테이블 조회"
description: "확률 테이블 조회"
---

# GetTables
public Task< GetRateTablesResult > **GetTablesAsync()**;  

## 설명
콘솔에 등록한 확률 카드 목록을 조회합니다.  
* 확률 파일을 선택하지 않은 확률는 리스트에서 제외됩니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## GetRateTablesResult
```js
public class RateTableInfo
{
    public string Name;
    public string Id;
    public string Explain;
    public string SelectedFileId;
    public string CreatedDate; 
}

public sealed class GetRateTablesResult : ListInfoResult<RateTableInfo>
{       
    public List<RateTableInfo> GetInfoList();
}
```


## Example

### Task 방식
```js
var reqResult = await BackndRate.Instance.GetTablesAsync();
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
BackndRate.Instance.GetTables(reqResult =>
{
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
    rows:
    [
        {
            "probabilityName": {
                "S": "확률 파일 이름"
            },
            "probabilityId": {
                "N": "600" // 확률 파일의 고유 아이디
            },
            "probabilityExplain": {
                "S": "확률 파일에 대한 부가설명입니다." // 설명이 없는 경우
            },
            "selectedProbabilityFileId": {
                "N": "1271" // 선택된 확률 테이블의 아이디
            },
            "createdDate": {
                "S": "2024-06-18T09:17:31.000Z" // 수정 날짜
            }
        },
        {
            "probabilityName": {
                "S": "Probability"
            },
            "probabilityId": {
                "N": "599"
            },
            "probabilityExplain": {
                "NULL": true
            },
            "selectedProbabilityFileId": {
                "N": "1270"
            },
            "createdDate": {
                "S": "2023-01-18T03:07:11.100Z"
            }
        },
        ...  
    ]
}
```
