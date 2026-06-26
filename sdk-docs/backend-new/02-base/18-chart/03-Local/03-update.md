---
sidebar_label: "최신 테이블로 업데이트"
description: "최신 테이블로 업데이트"
---

# Local.Update

public void **Update**(List< GameTableInfo > **tableList**, DownloadProgressDelegate **onProgress**, BackndBase.ResultCallback< GetGameContentsResult > **callback**);  
public Task< GetGameContentsResult > **UpdateAsync**(List< GameTableInfo > **tableList**, DownloadProgressDelegate **onProgress**);  

## 설명
인자값에 할당된 테이블 리스트와 로컬의 테이블 리스트를 비교하여 변경된 테이블만 불러와 로컬에 저장합니다.   
* 해당 함수는 SendQueue로 호출할 수 없습니다.

다음 테이블에 대해서만 새롭게 불러옵니다.
* 로컬에 존재하지 않은 테이블일 경우
* 선택한 테이블 파일이 변경되었을 경우
* 테이블을 수정했을 경우

:::note
뒤끝 콘솔에서 파일이 적용되지 않은 테이블이 로컬에 남아 있는 경우, `Update` 시 해당 로컬 테이블은 자동으로 삭제됩니다.  
:::
## Example

### Task 방식
```js
public async Task LocalUpdate()
{
    var getTableResult = await BackndGameTable.Instance.GetTablesAsync();
    if (getTableResult.IsSuccess() == false)
    {
        Debug.LogError(getTableResult);
        return;
    }
    
    var updateResult = await BackndGameTable.Instance.Local.UpdateAsync(getTableResult.GetInfoList(), null);
    if (updateResult.IsSuccess() == false)
    {
        Debug.LogError(updateResult);
        return;
    }

    var dic = updateResult.GetInfoById();
    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    //Table Id를 넣어주세요.
    if (dic.ContainsKey("Input ChartID"))
    {
        var json = dic["Input ChartID"].GetContentsJson();
        foreach (var item in json)
        {
            Debug.Log(item["itemID"]);
            Debug.Log(item["itemName"]);
            Debug.Log(item["hpPower"]);
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

public void LocalUpdateAsync()
{
    StartCoroutine(LocalUpdateAsyncIEnumerator());
}

IEnumerator LocalUpdateAsyncIEnumerator()
{
    GetGameTablesResult tableCallback = null;

    BackndGameTable.Instance.GetTables(bro =>
    {
        tableCallback = bro;
    });

    yield return new WaitUntil(() => tableCallback != null);

    if (tableCallback.IsSuccess() == false)
    {
        Debug.LogError(tableCallback);
        yield break;
    }

    GetGameContentsResult callback = null;
    BackndGameTable.Instance.Local.Update(tableCallback.GetInfoList(), GetProgress, bro =>
    {
        callback = bro;
    });

    yield return new WaitUntil(() => callback != null);

    if (callback.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback);
        yield break;
    }

    var dic = callback.GetInfoById();
    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    //Table Id를 넣어주세요.
    if (dic.ContainsKey("Input ChartID"))
    {
        var json = dic["Input ChartID"].GetContentsJson();
        foreach (var item in json)
        {
            Debug.Log(item["itemID"]);
            Debug.Log(item["itemName"]);
            Debug.Log(item["hpPower"]);
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
> 성공 시, GetInfoById() 함수를 확인해주세요.

### Contents
GameContentInfo 클래스의 Contents 는 다음의 예시에서는 아래와 같이 표시됩니다.

![image](/img/docs/guide/base/content/content/content-json.png)

```js
[
    {
        "itemID": "i101",
        "itemName": "아이템1",
        "hpPower": "1",
        "num": "1"
    },
    {
        "itemID": "i102",
        "itemName": "아이템2",
        "hpPower": "2",
        "num": "2"
    },
    ...
]
```
