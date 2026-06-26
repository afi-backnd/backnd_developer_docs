---
sidebar_label: "최신 차트로 업데이트"
---

# Backend.CDN.Content.Local.Update

public BackendContentReturnObject **Backend.CDN.Content.Local.Update**();  

public void **Backend.CDN.Content.Local.Update**(List< ContentTableItem > **contentTableList**, ContentProgressDelegate **contentProgressDelegate**, Backend.BackendCallback< BackendContentReturnObject > **backendCallback**);

## 설명
인자값에 할당된 차트 리스트와 로컬의 차트 리스트를 비교하여 변경된 차트만 불러와 로컬에 저장합니다.   
* 해당 함수는 SendQueue로 호출할 수 없습니다.

다음 차트에 대해서만 새롭게 불러옵니다.
* 로컬에 존재하지 않은 차트일 경우
* 선택한 차트 파일이 변경되었을 경우
* 차트를 수정했을 경우

:::note
뒤끝 콘솔에서 차트 파일이 적용되지 않은 차트가 로컬에 남아 있는 경우, `Update` 시 해당 로컬 차트는 자동으로 삭제됩니다.  
:::
## Example

### 동기
```js
public void LocalUpdate()
{
    BackEnd.Content.BackendContentTableReturnObject callback = Backend.CDN.Content.Table.Get();
    
    if(callback.IsSuccess() == false) {
        Debug.LogError(callback);
        return;
    }

    BackEnd.Content.BackendContentReturnObject localCallback = null;
    localCallback = Backend.CDN.Content.Local.Update(callback.GetContentTableItemList());

    if(localCallback.IsSuccess() == false) {
        Debug.LogError(localCallback);
        return;
    }

    Dictionary<string, BackEnd.Content.ContentItem> dic = localCallback.GetContentDictionarySortByChartId();
    
    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    // 차트 이름이 Content일 경우
    // contentJson은 아래 Success Cases를 참고해주세요
   if(dic.ContainsKey("Input ChartID"))  //Chart ID를 넣어주세요 *주의 :차트 파일 ID 아님*
    {
        LitJson.JsonData json = LitJson.JsonMapper.ToObject(dic["Input ChartID"].contentJson);

        foreach(LitJson.JsonData item in json) {
            Debug.Log(item["itemID"]);
            Debug.Log(item["itemName"]);
            Debug.Log(item["hpPower"]);
            Debug.Log(item["num"]);
        }
    }
}
```

### 비동기
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
    BackEnd.Content.BackendContentTableReturnObject tableCallback = null;
        
    Backend.CDN.Content.Table.Get(bro =>
    {
        tableCallback = bro;
    });
        
    yield return new WaitUntil(() => tableCallback != null);

    if(tableCallback.IsSuccess() == false) {
        Debug.LogError(tableCallback);
        yield break;
    }


    BackEnd.Content.BackendContentReturnObject callback = null;
        
    Backend.CDN.Content.Local.Update(tableCallback.GetContentTableItemList(), GetProgress, bro => {
        callback = bro;
    });
        
    yield return new WaitUntil(() => callback != null);

    if (callback.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback);
        yield break;
    }

    Dictionary<string, BackEnd.Content.ContentItem> dic = callback.GetContentDictionarySortByChartId();


    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    // 확률 파일 이름이 Content일 경우
    // contentJson은 아래 Success Cases를 참고해주세요
    if(dic.ContainsKey("Input ChartID"))  //Chart ID를 넣어주세요 *주의 :차트 파일 ID 아님*
    {
        LitJson.JsonData json = LitJson.JsonMapper.ToObject(dic["Input ChartID"].contentJson);

        foreach(LitJson.JsonData item in json) {
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
> 성공 시, GetContentDictionaryByChartName() 함수를 확인해주세요.

### ContentJson
ContentItem 클래스의 LitJson.JsonData contentJson는 다음의 예시 차트에서는 다음과 같이 표시됩니다.

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
