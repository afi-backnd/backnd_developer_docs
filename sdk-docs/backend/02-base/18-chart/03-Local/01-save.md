---
sidebar_label: "로컬 저장"
description: "Backend.CDN.Content.Local.Save"
---

# Backend.CDN.Content.Local.Save

public void **Backend.CDN.Content.Local.Save**(List< ContentItem > **contentList**,  Action< bool, Exception > **callback**);  
public bool **Backend.CDN.Content.Local.Save**(List< ContentItem > **contentList**,  out Exception **exception**);

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| contentTableList | List< ContentTableItem > | Backend.CDN.Content.Table.Get() 을 통해 불러올 수 있습니다. |


## 설명
차트를 로컬에 저장합니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example

### 동기
```js
void Save() 
{
    BackEnd.Content.BackendContentTableReturnObject callback;
    
    // 차트 테이블 불러오기
    callback = Backend.CDN.Content.Table.Get();
    
    if(callback.IsSuccess() == false) {
        Debug.LogError(callback);
        return;
    }

    BackEnd.Content.BackendContentReturnObject callback2 = null;

    // 불러온 차트 내용 조회하기
    callback2 = Backend.CDN.Content.Get(callback.GetContentTableItemList());
    
    if (callback2.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback2);
        return;
    }
    
    if(Backend.CDN.Content.Local.Save(callback2.GetContentList(), out Exception e) == false)
    {
        Debug.LogError("Save Error : " + e);
        return;
    }

    Debug.Log("로컬 저장에 성공했습니다");
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


    bool isDone = false;

    foreach (var item in callback2.GetContentList())
    {
        Debug.Log(item.chartName);
    }
    
    Backend.CDN.Content.Local.Save(callback2.GetContentList(), (isSuccess, exception) =>
    {
        isDone = true;
        if (isSuccess == false)
        {
            Debug.LogError("Local Save Fail : " + exception);
            return;
        }
    
        Debug.Log("Local Save Success");
    });
    
    yield return new WaitUntil(() => isDone);
}
```
