---
sidebar_label: 뽑기 n회 실행
description: "뽑기 n회 실행"
---

# Draw
public Task< DrawOnceResult > **DrawAsync**(string **fileId**, int **count**);  

## 파라미터

| Value        | Type           | Description  |  Default | Max |
| :------------ |:-------------| :----- | :----: | :----: |
| fileId | string | 확률 카드 파일의 id  | - | - |
| count | int | 한 번에 여러 개 받아올 횟수 | 1 | 100 |



## 설명
확률 카드를 이용하여 뽑기를 여러 번 수행합니다.  
* 해당 기능은 version2에서만 이용할 수 있습니다.  
* 최대 100회 뽑기를 수행할 수 있습니다.  
* 확률 차트의 percent에 해당하는 확률로 결과를 도출하여 리턴합니다. 
예를 들어, percent가 1인 경우 1퍼센트의 확률로 해당 결과가 리턴됩니다.  

> 확률의 소수점은 소수점 아래 최대 7자리까지 제공하고 있습니다.  


## Example

### Task 방식
```js
var reqResult = await BackndRate.Instance.DrawAsync("CardFileID", 10);
```

### Callback 방식
```js
BackndRate.Instance.Draw("CardFileID", 10, (callback) => 
{
    // 이후 처리 
});
```

## ReturnCase

### Success cases

**뽑기를 수행한 경우**  
statusCode : 200  
message : Success  
returnValue : RetrunValueJson 참조

### Error cases

**올바르지 못한 id를 입력한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad probability id, 잘못된 probability id 입니다

**확률에 소수점 8자리 이상의 데이터가 존재할 경우**  
statusCode : 500  
errorCode : ServerErrorException  
message : Expected number 'argument' '10000000000' failed predicate 'integer'

## RetrunValueJson
``` js
{
    elements:
    [
        {
            // 확률 차트의 num
            num: { S: "1" },
            // 발생할 확률
            percent: { S: "10" },
            // 확률 파일에 입력한 column
            compensation: { S: "present1" },
            // 확률 파일에 입력한 column
            monster: { S: "super" }
        },
        {
            num: [Object],
            percent: [Object],
            compensation: [Object],
            monster: [Object]
        }
        ..  
    ]
}
```

## Sample Code

```js
// 뒤끝의 기본 제공 차트를 이용하면 만든 아이템입니다.  
// 업로드하신 차트의 컬럼명에 맞게 변수를 변경해주시기 바랍니다
public class ProbabilityItem
{
    public string itemID;
    public string itemName;
    public string hpPower;
    public int num;
    public string percent;
    public override string ToString()
    {
        return $"itemID : {itemID}\n" +
        $"itemName : {itemName}\n" +
        $"hpPower : {hpPower}\n" +
        $"num : {num}\n" +
        $"percent : {percent}\n";
    }
}
```

```js
public async Task GetProbabilitysTest()
{
    string selectedProbabilityFileId = "93";

    // 10연차;
    var reqResult = await BackndRate.Instance.DrawAsync(selectedProbabilityFileId, 10);
    if (!reqResult.IsSuccess())
    {
        Debug.LogError(reqResult.ToString());
        return;
    }

    var json = reqResult.GetRows("elements");
    List<ProbabilityItem> itemList = new List<ProbabilityItem>();

    for (int i = 0; i < json.Count; i++)
    {
        ProbabilityItem item = new ProbabilityItem();

        item.itemID = json[i]["itemID"].ToString();
        item.itemName = json[i]["itemName"].ToString();
        item.hpPower = json[i]["hpPower"].ToString();
        item.num = int.Parse(json[i]["num"].ToString());
        item.percent = json[i]["percent"].ToString();

        itemList.Add(item);
    }

    foreach (var item in itemList)
    {
        Debug.Log(item.ToString());
    }
}
```
