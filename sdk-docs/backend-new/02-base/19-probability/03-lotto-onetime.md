---
sidebar_label: 뽑기 1회 실행
description: "뽑기 1회 실행"
---

# DrawOnce
public Task< DrawOnceResult > **DrawOnceAsync**(string **fileId**);  

## 파라미터

| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| fileId | string | 확률 카드 파일의 uuid 혹은 id |

## 설명
확률 카드를 이용하여 뽑기를 실행합니다.  
* 확률 차트의 percent에 해당하는 확률로 결과를 도출하여 리턴합니다. 
예를 들어, percent가 1인 경우 1퍼센트의 확률로 해당 결과가 리턴됩니다.  

> 확률의 소수점은 소수점 아래 최대 7자리까지 제공하고 있습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndRate.Instance.DrawOnceAsync("CardFileUuid");
```

### Callback 방식
```js
BackndRate.Instance.DrawOnce("CardFileUuid", (callback) => 
{
    // 이후 처리 
});
```

## ReturnCase

### Success cases

**뽑기를 수행한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

### Error cases

**올바르지 못한 uuid 혹은 id를 입력한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad **{selectedProbabilityCard uuid}{probability id}**, 잘못된 **{selectedProbabilityCard uuid/probability id}** 입니다

**확률에 소수점 8자리 이상의 데이터가 존재할 경우**  
statusCode : 400  
errorCode : BadParameterException  
message :bad decimal point 8, 잘못된 decimal point 8 입니다

## ReturnValueJson
``` js
{
    elements:
    {
        compensation:{ // 확률 파일에 입력한 column
            S:"present1"
        },
        percent:{ // 발생할 확률
            S:"10"
        },
        monster:{ // 확률 파일에 입력한 column
            S:"super"
        },
        num:{ // 확률 차트의 num
            S:"1"
        }
    }
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
public async Task GetProbabilityTest()
{
    string selectedProbabilityFileId = "93";

    var reqResult = await BackndRate.Instance.DrawOnceAsync(selectedProbabilityFileId);
    if (!reqResult.IsSuccess())
    {
        Debug.LogError(reqResult.ToString());
        return;
    }

    var json = reqResult.GetJObject();

    var item = new ProbabilityItem();
    item.itemID = json["elements"]["itemID"].ToString();
    item.itemName = json["elements"]["itemName"].ToString();
    item.hpPower = json["elements"]["hpPower"].ToString();
    item.num = int.Parse(json["elements"]["num"].ToString());
    item.percent = json["elements"]["percent"].ToString();

    Debug.Log(item.ToString());
}
```
