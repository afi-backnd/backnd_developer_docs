---
sidebar_label: "[Deprecated] 모든 차트 저장 V2"
description: "[Deprecated] 모든 차트 저장 V2"
---

# [Deprecated] GetAllChartAndSaveV2
public Task< RequestResult > **GetAllChartAndSaveV2Async**(bool **isChartKeyIsName**);

:::info 기능 개선 안내
기존 차트 기능의 속도를 대폭 개선한 CDN 차트 기능이 제공중에 있습니다.
조회/저장 과정에서 속도가 대폭 개선되었으니 해당 기능을 이용해 주세요.
:::

## 파라미터

| Value|  Type | Description |
| --- | --- | --- |
| isChartKeyIsName | bool | 저장할 차트의 키값으로 차트의 이름을 사용할지 여부  
**true** : 뒤끝 콘솔에서 지정한 차트의 이름을 키 값으로 사용  
**false** : 차트의 { uuid / id }를 키값으로 사용 |

## 설명
뒤끝 콘솔에 작성한 모든 차트에서 현재 적용한 파일을 불러와 저장합니다.  
- 입력한 파라미터에 따라 차트의 ID 혹은 뒤끝 콘솔에서 설정한 차트의 이름이 키로 적용됩니다.  
- 현재 적용된 차트 파일의 내용이 그 값으로 저장됩니다. 
- 현재 적용된 차트가 없다면 저장되지 않습니다.  


### GetAllChartAndSave와의 차이점
GetAllChartAndSave()와 기능은 거의 동일하지만 다음과 같은 차이점이 존재합니다.  
* 파일이 적용되지 않는 차트는 리스트에서 제외
* Json 리턴값중 old 컬럼 제거

:::caution GetAllChartAndSave 마이그레이션 안내
GetAllChartAndSave() 함수에서 GetAllChartAndSaveV2() 함수로 마이그레이션 시 다음과 같은 사항이 있는지 확인해주세요.  

- json["rows"][0]["S"]["old"].ToString()과 같이 JSON 파싱 중 old를 사용하는가.  

- 파일 적용이 되지 않은 차트에 대한 처리가 존재하는가.  
:::


## Example

### Task 방식
```js
var reqResult = await BackndLegacy.Chart.GetAllChartAndSaveV2Async(true);
```

### Callback 방식
```js
BackndLegacy.Chart.GetAllChartAndSaveV2(true, callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : RetunrValueJson 참조

## RetunrValueJson
```js
{
    rows:
    [
        {
            // 차트명
            chartName: { S: "몬스터 차트" },
            // 차트 설명
            chartExplain: { NULL: true },
            // 적용된 차트 파일 id(있는 경우)
            selectedChartFileId: { N: "47" },
        },
        {
            chartName: { S: "ItemChart" },
            chartExplain: { S : "아이템에 대한 정보가 들어가있는 차트입니다." },
            selectedChartFileId: { N: "47423" },
        }
    ]
}
```

## Sample Code
```js
public class ChartCardV2
{
    public string chartName; // 차트이름
    public string chartExplain; // 차트 설명
    public int selectedChartFileId;// 차트 파일 아이디

    public override string ToString()
    {
        return $"chartName: {chartName}\n" +
        $"chartExplain: {chartExplain}\n" +
        $"selectedChartFileId: {selectedChartFileId}\n";
    }
}
```

```js
public async Task GetAllChartAndSaveV2Test()
{
    var reqResult = await BackndLegacy.Chart.GetAllChartAndSaveV2Async(true);
    if (!reqResult.IsSuccess())
    {
        Debug.LogError("에러가 발생했습니다 : " + reqResult.ToString());
        return;
    }

    var chartCardList = new List<ChartCardV2>();
    var json = reqResult.GetRows();

    for (int i = 0; i < json.Count; i++)
    {
        ChartCardV2 chartCard = new ChartCardV2();

        chartCard.chartName = json[i]["chartName"].ToString();
        chartCard.chartExplain = json[i]["chartExplain"].ToString();
        chartCard.selectedChartFileId = json[i]["selectedChartFileId"].ToString();

        chartCardList.Add(chartCard);
    }

    foreach (var chartCard in chartCardList)
    {
        Debug.Log(chartCard.ToString() + "\n");
    }
}
```
