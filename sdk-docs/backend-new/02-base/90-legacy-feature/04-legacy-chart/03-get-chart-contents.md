---
sidebar_label: "[Deprecated] 차트 내용 조회"
---

# [Deprecated] GetChartContents
public Task< RequestResult >**GetChartContentsAsync**(string **chartFileId**);

:::info 기능 개선 안내
기존 차트 기능의 속도를 대폭 개선한 CDN 차트 기능이 제공중에 있습니다.  
조회/저장 과정에서 속도가 대폭 개선되었으니 해당 기능을 이용해 주세요.
:::

## 파라미터

| Value|  Type | Description |
| --- | --- |  --- |
| chartFileId| string | 차트 파일의 uuid 혹은 id   
 \[version1] : `reqResult.GetRows()[i]["selectedChartFile"]["uuid"]`  
 \[version2] : `reqResult.GetRows()[i]["selectedChartFileId"]`|

## 설명
콘솔에서 적용한 차트의 엑셀파일의 데이터를 조회합니다.  

> OLD 차트를 사용하는 경우 GetChartContents 함수를 호출하기 전에 반드시 GetChartList 함수를 호출해야 합니다.  


## Example

### Task 방식
```js
var reqResult = await BackndLegacy.Chart.GetChartContentsAsync("selectedChartFileId");
```

### Callback 방식
```js
BackndLegacy.Chart.GetChartContents("selectedChartFileId", (callback) =>
{
    // 이후 작업
});
```


## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue :  RetunrValueJson 참조

### Error case
**올바르지 못한 uuid / id를 입력한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad chart **uuid/id**, 잘못된 chart **uuid/id** 입니다

## RetunrValueJson
```js
{
    rows:
    [
        {
            num: { S: "1" }, // row num(번호)
            column1: { S: "contents1" },
            column2: { S: "contents2" },
            column3: { S: "contents3" }
        },
        {
            num: [Object],
            column1: [Object],
            column2: [Object],
            column3: [Object]
        }
    ]
}
```

## Sample Code

해당 예제코드는 뒤끝의 예제 차트를 이용하여 만든 class입니다.  
예제 차트에 대해서는 [뒤끝 콘솔가이드 - 차트 관리](https://developer.thebackend.io/guide/console-guide/backnd-base/chart/)를 참고해주세요.  

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
public async Task GetChartContentsTest()
{
    string selectedProbabilityFileId = "560";

    var reqResult = await BackndLegacy.Chart.GetChartContentsAsync(selectedProbabilityFileId);
    if (!reqResult.IsSuccess())
    {
        Debug.LogError(reqResult.ToString());
        return;
    }

    var json = reqResult.GetRows();
    var itemList = new List<ChartItem>();
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

    Debug.Log("차트 아이템의 총 갯수 : " + itemList.Count);
}
```
