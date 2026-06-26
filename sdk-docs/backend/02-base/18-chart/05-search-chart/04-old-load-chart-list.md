---
sidebar_label: "[Deprecated] 차트 리스트 조회"
draft: true
unlisted: true
---

# [Deprecated] GetChartList
public BackendReturnObject **GetChartList**();

## 설명
뒤끝 콘솔에 등록한 차트를 조회합니다.  
해당 차트는 뒤끝 콘솔의 차트 관리 항목에서 업로드하여 적용한 엑셀파일의 데이터입니다.  

조회 시 **차트의 명**과 **차트의 ID/UUID**, **차트의 버전**이 리턴되며, 차트의 데이터는 포함되어 있지 않습니다.  

## Example

### 동기
```js
Backend.Chart.GetChartList();
```

### 비동기
```js
Backend.Chart.GetChartList((callback) => 
{
    // 이후 작업
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.Chart.GetChartList, (callback) => 
{
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
        // version 1(old)
        // selectedChartFile 이 없는 경우
        {
            // 차트 uuid
            uuid: { S: "538b3a20-7b7a-11e8-8002-f31a1dd37719" },
            // 차트 indate
            inDate: { S: "2018-06-29T08:56:35.266Z" },
            // 차트 설명
            chartExplain: { S: "2" },
            // 차트명
            chartName: { S: "v1" },
            // version 정보(y: version1 , n: version2)
            old: { S: "y" }
        },
        // version 1(old)
        // selectedChartFile 이 있는 경우
        {
            // 차트에 적용한 파일 정보
            selectedChartFile:
            {
                M:
                {
                    // 차트의 row 수
                    count: { N: "1000" },
                    // 차트 파일 uuid
                    uuid: { S: "780932f0-75fb-11e8-bf7a-cbcc37090d69" },
                    // 차트 파일 indate
                    inDate: { S: "2018-06-22T09:05:54.591Z" },
                    // 차트 파일 명
                    chartFileName: { S: "222222.xlsx" }
                }
            },
            // 차트 indate
            inDate: { S: "2018-06-22T09:05:38.562Z" },
            // 차트 uuid
            uuid: { S: "6e7b5e20-75fb-11e8-bf7a-cbcc37090d69" },
            // 차트 설명
            chartExplain: { S: "v1" },
            // 차트명(이 이름으로 PlayerPrefs에 selectedChartFile에 대한 차트 내용이 저장됨)
            chartName: { S: "23" },
            // version 정보(y: version1 , n: version2)
            old: { S: "y" }
        },
        // version 2(new)
        // selectedChartFile 이 없는 경우
        {
            // 차트명
            chartName: { S: "gggg" },
            // 차트 설명
            chartExplain: { NULL: true },
            // 적용된 차트 파일 id(없는 경우)
            selectedChartFileId: { NULL: true },
            // version 정보(y: version1 , n: version2)
            old: { S: "n" }
        },
        // version 2(new)
        // selectedChartFile 이 있는 경우
        {
            // 차트명
            chartName: { S: "ㅎㅇㅎㅇ" },
            // 차트 설명
            chartExplain: { NULL: true },
            // 적용된 차트 파일 id(있는 경우)
            selectedChartFileId: { N: "47" },
            // version 정보(y: version1 , n: version2)
            old: { S: "n" }
        }
    ]
}
```

## Sample Code
```js
public class ChartCard
{
    public bool isChartUpload = true; //차트가 적용되어있는지(returnValue에는 없는 값)
    public string chartName; // 차트이름
    public string chartExplain; // 차트 설명
    public int selectedChartFileId;// 차트 파일 아이디
    public string old; // 신규버전인지

    public override string ToString()
    {
        return $"chartName: {chartName}\n" +
        $"chartExplain: {chartExplain}\n" +
        $"isChartUpload: {isChartUpload}\n" +
        $"selectedChartFileId: {selectedChartFileId}\n" +
        $"old: {old}\n";
    }
}

public void GetChartListTest()
{
    var bro = Backend.Chart.GetChartList();

    if(!bro.IsSuccess())
    {
        Debug.LogError("에러가 발생했습니다 : " + bro.ToString());
        return;
    }

    List<ChartCard> chartCardList = new List<ChartCard>();

    LitJson.JsonData json = bro.FlattenRows();

    for(int i = 0; i < json.Count; i++)
    {
        ChartCard chartCard = new ChartCard();

        chartCard.chartName = json[i]["chartName"].ToString();
        chartCard.chartExplain = json[i]["chartExplain"].ToString();
        int outNum = 0;

        if(int.TryParse(json[i]["selectedChartFileId"].ToString(), out outNum))
        {
            chartCard.isChartUpload = true;
            chartCard.selectedChartFileId = outNum;
        }
        else
        {
            chartCard.isChartUpload = false;
            chartCard.selectedChartFileId = 0;
        }

        chartCard.old = json[i]["old"].ToString();

        chartCardList.Add(chartCard);
    }

    foreach(var chartCard in chartCardList)
    {
        Debug.Log(chartCard.ToString() + "\n");
    }
}
```
