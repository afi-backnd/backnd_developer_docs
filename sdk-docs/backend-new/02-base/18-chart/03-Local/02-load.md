---
sidebar_label: "로컬 불러오기"
description: "로컬 불러오기"
---

# Local.Load

public GetGameContentsResult **Load**();  
public void **Load**(BackndBase.ResultCallback< GetGameContentsResult > **callback**);  
public Task< GetGameContentsResult > **LoadAsync**();

## 설명
로컬에 저장된 테이블들을 불러옵니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example

### Task 방식
```js
public async Task Load()
{        
    var loadResult = await BackndGameTable.Instance.Local.LoadAsync();
    if (loadResult.IsSuccess() == false)
    {
        Debug.LogError(loadResult);
        return;
    }

    var dic = loadResult.GetInfoById();
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
public void LoadAsync()
{
    StartCoroutine(LoadAsyncIEnumerator());
}

IEnumerator LoadAsyncIEnumerator()
{
    GetGameContentsResult callback = null;
    BackndGameTable.Instance.Local.Load(result => {
        callback = result;
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
