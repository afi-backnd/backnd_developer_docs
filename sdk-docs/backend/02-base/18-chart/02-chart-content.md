---
sidebar_label: "차트 내용 조회"
description: "Backend.CDN.Content.Get"
---

# Backend.CDN.Content.Get
public BackendContentReturnObject **Backend.CDN.Content.Get**(List< ContentTableItem > **contentTableList**);  

public void **Backend.CDN.Content.Get**(List< ContentTableItem > **contentTableList**, ContentProgressDelegate **contentProgressDelegate**, Backend.BackendCallback< BackendContentReturnObject > **backendCallback**);

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| contentTableList | List< ContentTableItem > | Backend.CDN.Content.Table.Get() 을 통해 불러올 수 있습니다. |
| contentProgressDelegate | ContentProgressDelegate | 파일 다운로드의 진행상황을 알려주는 delegate 함수 |

## 설명
Backend.CDN.Content.Table.Get()를 통해 불러온 List< ContentTableItem >에 등록된 확률들의 내용을 불러옵니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

리턴값이 204 성공 시, BackendContentReturnObject에 존재하는 **List< ContentItem > GetContentList()**에서 해당 파일의 값을 Json으로 확인하실 수 있습니다.  

### ContentProgressDelegate

```js
public delegate void ContentProgressDelegate(int totalCount, int remainCount, string downloadFile);
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

Backend.CDN.Content.Get(callback.GetContentTableItemList(), GetProgress, bro =>
{
    Debug.Log(bro);
});

```

### BackendContentReturnObject
```js
namespace BackEnd.Content
{
    public class ContentItem
    {
        public string chartName;
        public string chartId;
        public string createdDate;
        public string selectedChartFileId;
        public string errorString; // 파일 다운로드 시 에러가 발생할 경우 값이 할당됩니다.
        public string contentString { get; internal set; }

        public LitJson.JsonData contentJson
        {
            get
            {
                if (contentJson == null)
                {
                    contentJson = new LitJson.JsonData(contentString);
                }

                return contentJson;
            }

            private set => contentJson = value;
        }
    }

    public class BackendContentReturnObject : BackendReturnObject
    {
        public Dictionary<string, ContentItem> GetContentDictionarySortByChartId();

        public List<ContentItem> GetContentList();
    }
}
```


## Example

### 동기
```js
void GetContent() 
{
    BackEnd.Content.BackendContentTableReturnObject callback;
        
    // 차트 테이블 불러오기
    callback = Backend.CDN.Content.Table.Get();
        
    if(callback.IsSuccess() == false) {
        Debug.LogError(callback);
        return;
    }

    BackEnd.Content.BackendContentReturnObject callback2 = null;
        

    // 불러온 차트의 내용 조회하기
    callback2 = Backend.CDN.Content.Get(callback.GetContentTableItemList());
        
    if (callback2.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback2);
        return;
    }

    // 성공 시, Dictionary 형태로 변환
    Dictionary<string, BackEnd.Content.ContentItem> dic = callback2.GetContentDictionarySortByChartId();

    // 내용 확인
    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

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

public void GetAsync()
{
    StartCoroutine(GetAsyncIEnumerator());
}

IEnumerator GetAsyncIEnumerator()
{
    BackEnd.Content.BackendContentTableReturnObject callback = null;
    
    Backend.CDN.Content.Table.Get(bro =>
    {
        callback = bro;
    });
    
    yield return new WaitUntil(() => callback != null);

    if(callback.IsSuccess() == false) {
        Debug.LogError(callback);
        yield break;
    }

    BackEnd.Content.BackendContentReturnObject callback2 = null;
    
    Backend.CDN.Content.Get(callback.GetContentTableItemList(), GetProgress, bro =>
    {
        callback2 = bro;
    });
    
    yield return new WaitUntil(() => callback2 != null);

    if (callback2.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback2);
        yield break;
    }

    Dictionary<string, BackEnd.Content.ContentItem> dic = callback2.GetContentDictionarySortByChartId();


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
