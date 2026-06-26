---
sidebar_label: "뽑기 확률 내용 조회"
---

# GetContents
public Task< GetRateContentsResult > **GetContentsAsync**(List< RateTableInfo > **tableInfoList**, DownloadProgressDelegate **onProgress**);  

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| tableInfoList | List< RateTableInfo > | BackndRate.Instance.GetTables() 을 통해 불러올 수 있습니다. |
| onProgress | DownloadProgressDelegate | 파일 다운로드의 진행상황을 알려주는 delegate 함수 |

## 설명
BackndRate.Instance.GetTables()를 통해 불러온 List< RateTableInfo >에 등록된 확률들의 내용을 불러옵니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

리턴값이 204 성공 시, GetRateContentsResult에 존재하는 **List< RateTableInfo > GetInfoList()**에서 해당 파일의 값을 Json으로 확인하실 수 있습니다.  

### DownloadProgressDelegate

```js
public delegate void DownloadProgressDelegate(int totalCount, int remainCount, string downloadFile);
```

해당 delegate 함수의 경우, 함수 내에서 다운로드가 완료된 후 다음 다운로드파일을 진행할 때마다 호출되는 함수입니다.  
해당 함수로 현재 다운로드 진행 상황을 보여줄 수 있습니다.

다음과 같이 이용하실 수 있습니다.
```js
private void GetProgress(int totalCount, int remainCount, string fileName)
{
    Debug.Log("총 파일 갯수 : " + totalCount);
    Debug.Log("남은 파일 갯수 : " + remainCount);
    Debug.Log("현재 다운로드할 파일 이름 : " + fileName);
}

BackndRate.Instance.GetContents(callback.GetInfoList(), GetProgress, reqResult =>
{
    Debug.Log(reqResult);
});
```

### GetRateContentsResult
```js
public class RateContentInfo
{
    public string TableName;
    public string TableId;
    public string CreatedDate;
    public string SelectedFileId;
    public string ErrorText;
    public string Contents;

    public JArray GetContentsJson();
}

public sealed class GetRateContentsResult : RequestResult
{
    public Dictionary<string, RateContentInfo> GetInfoSetById();
    public List<RateContentInfo> GetInfoList();    
}
```


## Example

### Task 방식
```js
async Task GetProbability() 
{
    // 불러온 확률 테이블의 내용 조회하기
    var reqResult = await BackndRate.Instance.GetContentsAsync(callback.GetInfoList(), null);
    if (reqResult.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + reqResult);
        return;
    }

    // 성공 시, Dictionary 형태로 변환
    var dic = reqResult.GetInfoSetById();

    // 내용 확인
    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    if (dic.ContainsKey("Probability"))
    {
        var json = dic["Probability"].GetContentsJson();
        foreach (var item in json)
        {
            Debug.Log(item["itemID"]);
            Debug.Log(item["itemName"]);
            Debug.Log(item["hpPower"]);
            Debug.Log(item["percent"]);
            Debug.Log(item["num"]);
        }
    }
}
```

### Callback 방식
```js
private void GetProgress(int totalCount, int remainCount, string fileName)
{
    Debug.Log("totalCount : " + totalCount + " remainCount : " + remainCount + " fileName : " + fileName);
}

public void GetProbabilityAsync()
{
    StartCoroutine(GetProbabilityAsyncIEnumerator());
}

IEnumerator GetProbabilityAsyncIEnumerator()
{
    GetRateTablesResult callback = null;
    BackndRate.Instance.GetTables(reqResult =>
    {
        callback = reqResult;
    });

    yield return new WaitUntil(() => callback != null);

    if (callback.IsSuccess() == false)
    {
        Debug.LogError(callback);
        yield break;
    }

    GetRateContentsResult callback2 = null;
    BackndRate.Instance.GetContents(callback.GetInfoList(), GetProgress, reqResult =>
    {
        callback2 = reqResult;
    });

    yield return new WaitUntil(() => callback2 != null);

    if (callback2.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback2);
        yield break;
    }

    // 성공 시, Dictionary 형태로 변환
    var dic = callback2.GetInfoSetById();

    // 내용 확인
    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    if (dic.ContainsKey("Probability"))
    {
        var json = dic["Probability"].GetContentsJson();
        foreach (var item in json)
        {
            Debug.Log(item["itemID"]);
            Debug.Log(item["itemName"]);
            Debug.Log(item["hpPower"]);
            Debug.Log(item["percent"]);
            Debug.Log(item["num"]);
        }
    }
}
```


## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 204  
message : Success  
> 성공 시, GetInfoSetById() 함수를 확인해주세요.

### Contents
RateContentInfo 클래스의 Contents 는 다음의 예시 차트에서는 다음과 같이 표시됩니다.

![image](/img/docs/guide/base/content/probability/content-json.png)

```js
[
    {
        "itemID": "i101",
        "itemName": "아이템1",
        "hpPower": "1",
        "percent": "3.5",
        "num": "1"
    },
    {
        "itemID": "i102",
        "itemName": "아이템2",
        "hpPower": "2",
        "percent": "3.5",
        "num": "2"
    },
    ...
]
```
