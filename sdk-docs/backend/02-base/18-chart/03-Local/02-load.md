---
sidebar_label: "로컬 불러오기"
description: "Backend.CDN.Content.Local.Load"
---

# Backend.CDN.Content.Local.Load

public BackendContentReturnObject **Backend.CDN.Content.Local.Load**();  
public void **Backend.CDN.Content.Local.Load**(Backend.BackendCallback< BackendContentReturnObject > **backendCallback**);  

## 설명
로컬에 저장된 차트들을 불러옵니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example

### 동기
```js
public void Load()
{
    BackEnd.Content.BackendContentReturnObject localCallback = null;
    localCallback = Backend.CDN.Content.Local.Load();

    if(localCallback.IsSuccess() == false) {
        Debug.LogError(localCallback);
        return;
    }

    Dictionary<string, BackEnd.Content.ContentItem> dic = localCallback.GetContentDictionarySortByChartId();


    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    // 차트 파일 이름이 Content일 경우
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
public void LoadAsync()
{
    StartCoroutine(LoadAsyncIEnumerator());
}

IEnumerator LoadAsyncIEnumerator()
{

    BackEnd.Content.BackendContentReturnObject callback = null;
        
    Backend.CDN.Content.Local.Load(bro => {
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
> 성공 시, GetContentDictionary() 함수를 확인해주세요.

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
