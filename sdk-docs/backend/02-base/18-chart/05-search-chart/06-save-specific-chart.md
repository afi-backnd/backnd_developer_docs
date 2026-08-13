---
sidebar_label: "[Deprecated] 특정 차트 저장"
draft: "true"
unlisted: "true"
description: "[Deprecated] GetOneChartAndSave"
---

# [Deprecated] GetOneChartAndSave
public BackendReturnObject **GetOneChartAndSave**(string **chartFileId**);  
public BackendReturnObject **GetOneChartAndSave**(string **chartFileId**, string **chartName**);

:::info 기능 개선 안내
기존 차트 기능의 속도를 대폭 개선한 CDN 차트 기능이 제공중에 있습니다.  
조회/저장 과정에서 속도가 대폭 개선되었으니 해당 기능을 이용해 주세요.
:::

## 파라미터

| Value|  Type | Description |
| --- | --- | --- |
| chartFileId | string |  차트 파일의 uuid 혹은 id|
| chartName| string | (Optional) 로컬에 저장될 차트 파일 이름. 지정하지 않은 경우 chartFileId로 저장됩니다.|

## 설명
뒤끝 콘솔에 등록한 차트를 불러와 저장합니다.  
차트의 저장은 [뒤끝 파일 시스템 기능](/sdk-docs/backend/base/sdk-utils/filesystem)을 이용하여 기기 로컬에 저장합니다.  
해당 차트는 뒤끝 콘솔의 차트 관리 항목에서 업로드하여 적용한 엑셀파일의 데이터입니다.  

### 차트 하나를 불러오고 저장하기
뒤끝 콘솔에 작성한 차트에서 현재 적용한 파일을 불러와 저장합니다.  
- 입력한 파라미터에 따라 차트의 ID 혹은 별도로 지정한 차트 이름이 키로 적용됩니다.  
- 현재 적용된 차트 파일의 내용이 그 값으로 저장됩니다. 
- 현재 적용된 차트가 없다면 저장되지 않습니다.  

## Example

### 동기
```js
Backend.Chart.GetOneChartAndSave("chartFileId");
Backend.Chart.GetOneChartAndSave("chartFileId", "차트 이름");
```

### 비동기
```js
Backend.Chart.GetOneChartAndSave("uuid", callback =>
{
    // 이후 처리
});
Backend.Chart.GetOneChartAndSave("uuid", "차트 이름", callback =>
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.Chart.GetOneChartAndSave,"uuid", callback =>
{
    // 이후 처리
});
SendQueue.Enqueue(Backend.Chart.GetOneChartAndSave,"uuid", "차트 이름", callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**올바르지 못한 uuid /id를 입력한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad chart **uuid/id**, 잘못된 chart **uuid/id**입니다

## GetReturnValuetoJSON
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

public void GetOneChartAndSaveTest()
{
    string selectedProbabilityFileId = "560";

    var bro = Backend.Chart.GetOneChartAndSave(selectedProbabilityFileId);

    if(!bro.IsSuccess())
    {
        Debug.LogError(bro.ToString());
        return;
    }

    LitJson.JsonData json = bro.FlattenRows();

    List<ChartItem> itemList = new List<ChartItem>();

    for(int i = 0; i < json.Count; i++)
    {
        ChartItem item = new ChartItem();

        item.itemID = json[i]["itemID"].ToString();
        item.itemName = json[i]["itemName"].ToString();
        item.hpPower = json[i]["hpPower"].ToString();

        itemList.Add(item);
    }

    foreach(var item in itemList)
    {
        Debug.Log(item.ToString());
    }

    Debug.Log("차트 아이템의 총 갯수 : " + itemList.Count);
}
```
