---
sidebar_label: "로컬 저장"
description: "로컬 저장"
---

# Local.Save

public bool **Save**(List< GameContentInfo > **contentList**, out Exception **e**);  
public void **Save**(List< GameContentInfo > **contentList**,  Action< bool, Exception > **callback**);  
public Task< (bool, Exception) > **SaveAsync**(List< GameContentInfo > **contentList**,  out Exception **exception**);

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| contentList | List< GameContentInfo > | BackndGameTable.Instance.GetContents() 을 통해 불러올 수 있습니다. |


## 설명
테이블을 로컬에 저장합니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

## Example

### Task 방식
```js
async Task Save()
{
    // 게임 테이블 불러오기
    var getTableResult = await BackndGameTable.Instance.GetTablesAsync();
    if (getTableResult.IsSuccess() == false)
    {
        Debug.LogError(getTableResult);
        return;
    }

    // 불러온 테이블 내용 조회하기
    var contentResult = await BackndGameTable.Instance.GetContentsAsync(getTableResult.GetInfoList(), null);
    if (contentResult.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + contentResult);
        return;
    }

    var saveResult = await BackndGameTable.Instance.Local.SaveAsync(contentResult.GetInfoList());
    if (saveResult.Item1 == false)
    {
        Debug.LogError("Save Error : " + saveResult.Item2);
        return;
    }

    Debug.Log("로컬 저장에 성공했습니다");
}
```

### Callback 방식
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
    GetGameTablesResult callback = null;
    BackndGameTable.Instance.GetTables(result =>
    {
        callback = result;
    });

    yield return new WaitUntil(() => callback != null);

    if (callback.IsSuccess() == false)
    {
        Debug.LogError(callback);
        yield break;
    }

    GetGameContentsResult callback2 = null;
    BackndGameTable.Instance.GetContents(callback.GetInfoList(), GetProgress, result =>
    {
        callback2 = result;
    });

    yield return new WaitUntil(() => callback2 != null);

    if (callback2.IsSuccess() == false)
    {
        Debug.LogError("GetContents : Fail : " + callback2);
        yield break;
    }


    bool isDone = false;
    foreach (var info in callback2.GetInfoList())
    {
        Debug.Log(info.TableId);
    }

    BackndGameTable.Instance.Local.Save(callback2.GetInfoList(), (isSuccess, exception) =>
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
