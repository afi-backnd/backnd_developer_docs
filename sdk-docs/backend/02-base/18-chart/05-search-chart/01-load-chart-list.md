---
sidebar_label: "[Deprecated] 차트 리스트 불러오기 V2"
draft: "true"
unlisted: "true"
description: "[Deprecated] GetChartListV2"
---

# [Deprecated] GetChartListV2
public BackendReturnObject **GetChartListV2**();

:::info 기능 개선 안내
기존 차트 기능의 속도를 대폭 개선한 CDN 차트 기능이 제공중에 있습니다.  
조회/저장 과정에서 속도가 대폭 개선되었으니 해당 기능을 이용해 주세요.
:::
## 설명
뒤끝 콘솔에 등록한 차트를 조회합니다.  
해당 차트는 뒤끝 콘솔의 차트 관리 항목에서 업로드하여 적용한 엑셀파일의 데이터입니다.  

조회 시 **차트의 명**과 **차트의 ID/UUID**, **차트의 버전**이 리턴되며, 차트의 데이터는 포함되어 있지 않습니다.  

### GetChartList와의 차이점
GetChartList와 기능은 거의 동일하지만 다음과 같은 차이점이 존재합니다.  
* 파일이 적용되지 않는 차트는 리스트에서 제외
* Json 리턴값중 old 컬럼 제거

:::caution GetChartList 마이그레이션 안내
GetChartList() 함수에서 GetChartListV2() 함수로 마이그레이션 시 다음과 같은 사항이 있는지 확인해주세요.  

- json["rows"][0]["S"]["old"].ToString()과 같이 JSON 파싱 중 old를 사용하는가.  

- 파일 적용이 되지 않은 차트에 대한 처리가 존재하는가.  
:::

## Example

### 동기
```js
Backend.Chart.GetChartListV2();
```

### 비동기
```js
Backend.Chart.GetChartListV2((callback) => {
    // 이후 작업
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.Chart.GetChartListV2, (callback) => {
    // 이후 작업
});
```

## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON
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
public void GetChartListV2Test()
{
    var bro = Backend.Chart.GetChartListV2();

    if(!bro.IsSuccess())
    {
        Debug.LogError("에러가 발생했습니다 : " + bro.ToString());
        return;
    }

    List<ChartCardV2> chartCardList = new List<ChartCardV2>();

    LitJson.JsonData json = bro.FlattenRows();

    for(int i = 0; i < json.Count; i++)
    {
        ChartCardV2 chartCard = new ChartCardV2();

        chartCard.chartName = json[i]["chartName"].ToString();
        chartCard.chartExplain = json[i]["chartExplain"].ToString();
        chartCard.selectedChartFileId= json[i]["selectedChartFileId"].ToString();

        chartCardList.Add(chartCard);
    }

    foreach(var chartCard in chartCardList)
    {
        Debug.Log(chartCard.ToString() + "\n");
    }
}
```
