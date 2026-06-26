---
sidebar_label: "[Deprecated] 뽑기 확률 내용 조회"
description: "[Deprecated] 뽑기 확률 내용 조회"
---

# [Deprecated] GetProbabilityContents
public Task< RequestResult > **GetProbabilityContentsAsync**(string **chartFileId**);

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| chartFileId| string | 확률 파일의 id   

## 설명
콘솔에 등록된 확률 차트의 파일 내용을 조회합니다.

## Example

### Task 방식
```js
var reqResult = await BackndLegacy.Probability.GetProbabilityContentsAsync("fileId");
```

### Callback 방식
```js
BackndLegacy.Probability.GetProbabilityContents("fileId", (callback) =>
{
    // 이후 작업
});
```


## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue :  ReturnValueJson 참조

### Error case
**올바르지 못한 fileId를 입력한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : probability not found, probability을(를) 찾을 수 없습니다  

**fileId가 string.Empty이거나 null인 경우**  
statusCode : 400  
errorCode : NullParameterException  
message : fileId is null or Empty  

## ReturnValueJson
```js
{
    rows: [
        {
            itemID : "i101",
            itemName : "아이템1",
            hpPower : "1",
            percent : "3.412345679",
            num : "1"
        },
        {
            itemID : "i102",
            itemName : "아이템2",
            hpPower : "2",
            percent : "3.587654321",
            num : "2"
        }
    ]
}
```

## Sample Code

해당 예제코드는 뒤끝의 예제 차트를 이용하여 만든 class입니다.  
예제 차트에 대해서는 [뒤끝 콘솔가이드 - 차트 관리](https://docs.thebackend.io/guide/console-guide/backnd-base/chart/)를 참고해주세요.  

```js
// 뒤끝의 기본 제공 차트를 이용하면 만든 아이템입니다.  
// 업로드하신 차트의 컬럼명에 맞게 변수를 변경해주시기 바랍니다
public class ChartItem
{
    public string itemID;
    public string itemName;
    public string hpPower;
    public string percent;

    public override string ToString()
    {
        return $"itemID : {itemID}\n" +
        $"itemName : {itemName}\n" +
        $"hpPower : {hpPower}\n";
    }
}
```

```js
public async Task GetProbabilityContentsTest()
{
    string probabilityFileId = "fileId";

    var reqResult = await BackndLegacy.Probability.GetProbabilityContentsAsync(probabilityFileId);
    if (!reqResult.IsSuccess())
    {
        Debug.LogError(reqResult.ToString());
        return;
    }

    var json = reqResult.GetRows();
    List<ChartItem> itemList = new List<ChartItem>();

    for (int i = 0; i < json.Count; i++)
    {
        ChartItem item = new ChartItem();

        item.itemID = json[i]["itemID"].ToString();
        item.itemName = json[i]["itemName"].ToString();
        item.hpPower = json[i]["hpPower"].ToString();

        itemList.Add(item);
    }

    foreach (var item in itemList)
    {
        Debug.Log(item.ToString());
    }

    Debug.Log("확률 아이템의 총 갯수 : " + itemList.Count);
}
```
