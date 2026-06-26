---
sidebar_label: "뽑기 확률 내용 조회"
---

# Backend.CDN.Probability.Get
public BackendProbabilityContentReturnObject **Backend.CDN.Probability.Get**(List< ProbabilityTableItem > **probabilityTableList**);  

public void **Backend.CDN.Probability.Get**(List< ProbabilityTableItem > **probabilityTableList**, ContentProgressDelegate **contentProgressDelegate**, Backend.BackendCallback< BackendProbabilityContentReturnObject > **backendCallback**);

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| probabilityTableList | List< ProbabilityTableItem > | Backend.CDN.Probability.Table.Get() 을 통해 불러올 수 있습니다. |
| contentProgressDelegate | ContentProgressDelegate | 파일 다운로드의 진행상황을 알려주는 delegate 함수 |

## 설명
Backend.CDN.Probability.Table.Get()를 통해 불러온 List< ProbabilityTableItem >에 등록된 확률들의 내용을 불러옵니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

리턴값이 204 성공 시, BackendProbabilityContentReturnObject에 존재하는 **List< ProbabilityContentItem > GetProbabilityContentList()**에서 해당 파일의 값을 Json으로 확인하실 수 있습니다.  

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

Backend.CDN.Probability.Get(callback.GetProbabilityTableItemList(), GetProgress, bro =>
{
    Debug.Log(bro);
});

```

### BackendProbabilityContentReturnObject
```js
namespace BackEnd.ProbabilityContent
{
    public class ProbabilityContentItem
    {
        public string probabilityName;
        public string probabilityId;
        public string selectedProbabilityFileId;
        public string errorString;
        public string createdDate;
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

    public class BackendProbabilityContentReturnObject : BackendReturnObject
    {
        // GetProbabilityContentDictionarySortByProbabilityName의 경우, 확률 이름이 동일할 경우, key값을 ProbabilityId로 변환하여 저장합니다.
        public Dictionary<string, ProbabilityContentItem> GetProbabilityContentDictionarySortByProbabilityName();

        public Dictionary<string, ProbabilityContentItem> GetProbabilityContentDictionarySortByProbabilityId();

        public List<ProbabilityContentItem> GetProbabilityContentList();
    }
}
```


## Example

### 동기
```js
void GetProbability() 
{
    BackEnd.ProbabilityContent.BackendProbabilityTableReturnObject callback;
        
    // 확률 테이블 불러오기
    callback = Backend.CDN.Probability.Table.Get();
        
    if(callback.IsSuccess() == false) {
        Debug.LogError(callback);
        return;
    }

    BackEnd.ProbabilityContent.BackendProbabilityContentReturnObject callback2 = null;
        

    // 불러온 확률 테이블의 내용 조회하기
    callback2 = Backend.CDN.Probability.Get(callback.GetProbabilityTableItemList());
        
    if (callback2.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback2);
        return;
    }

    // 성공 시, Dictionary 형태로 변환
    Dictionary<string, BackEnd.ProbabilityContent.ProbabilityContentItem> dic = callback2.GetProbabilityContentDictionarySortByProbabilityName();

    // 내용 확인
    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    // contentJson은 아래 Success Cases를 참고해주세요
    if(dic.ContainsKey("Probability")) 
    {

        LitJson.JsonData json = dic["Probability"].contentJson;

        foreach(LitJson.JsonData item in json) {
            Debug.Log(item["itemID"]);
            Debug.Log(item["itemName"]);
            Debug.Log(item["hpPower"]);
            Debug.Log(item["percent"]);
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

public void GetProbabilityAsync()
{
    StartCoroutine(GetProbabilityAsyncIEnumerator());
}

IEnumerator GetProbabilityAsyncIEnumerator()
{
    BackEnd.ProbabilityContent.BackendProbabilityTableReturnObject callback = null;
    
    Backend.CDN.Probability.Table.Get(bro =>
    {
        callback = bro;
    });
    
    yield return new WaitUntil(() => callback != null);

    if(callback.IsSuccess() == false) {
        Debug.LogError(callback);
        yield break;
    }

    BackEnd.ProbabilityContent.BackendProbabilityContentReturnObject callback2 = null;
    
    Backend.CDN.Probability.Get(callback.GetProbabilityTableItemList(), GetProgress, bro =>
    {
        callback2 = bro;
    });
    
    yield return new WaitUntil(() => callback2 != null);

    if (callback2.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback2);
        yield break;
    }

    Dictionary<string, BackEnd.ProbabilityContent.ProbabilityContentItem> dic = callback2.GetProbabilityContentDictionarySortByProbabilityName();


    foreach (string keyName in dic.Keys)
    {
        Debug.Log(dic[keyName].ToString());
    }

    // 확률 파일 이름이 Probability일 경우
    // contentJson은 아래 Success Cases를 참고해주세요
    if(dic.ContainsKey("Probability")) {

        LitJson.JsonData json = dic["Probability"].contentJson;

        foreach(LitJson.JsonData item in json) {
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
> 성공 시, GetProbabilityContentDictionary() 함수를 확인해주세요.

### ContentJson
ProbabilityContentItem 클래스의 LitJson.JsonData contentJson는 다음의 예시 차트에서는 다음과 같이 표시됩니다.

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
