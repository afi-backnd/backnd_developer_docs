---
sidebar_label: "모든 길드 랭킹 설정값 정보 조회"
draft: "true"
unlisted: "true"
description: "GetRankTableList"
---

# GetRankTableList

public BackendReturnObject **GetRankTableList**();  

:::info 기능 개선 안내
기존 랭킹의 성능과 기능을 개선한 **리더보드** 기능을 제공하고 있습니다.  
**콘솔에서 신규 생성은 리더보드만 제공**하므로 해당 기능을 사용해 주세요.
:::

## 설명

뒤끝 콘솔에 생성한 모든 길드 랭킹을 리스트 형태로 리턴합니다.  
해당 리스트에는 아래 정보가 포함되어 있습니다.  
* 랭킹 이름
* 랭킹 uuid
* 랭킹 주기
* 랭킹 초기화 시 데이터 초기화 여부
* 랭킹 정렬 방법
* 랭킹에 사용한 굿즈/메타 정보

## Example

### 동기
```js
Backend.URank.Guild.GetRankTableList();
```

### 비동기
```js
Backend.URank.Guild.GetRankTableList(callback=>
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue(Backend.URank.Guild.GetRankTableList, callback => 
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

**랭킹이 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

## GetReturnValuetoJSON
```js
{
    "rows": [
        {
            // 랭크 타입. 길드 랭킹의 경우 guild
            "rankType": {
                "S": "guild"
            },
            // 일회성 랭킹에서만 존재합니다.  
            // 랭킹 시작일
            "rankStartDateAndTime": {
                "S": "2021-03-14T15:00:00.000Z"
            },
            // 일회성 랭킹에서만 존재합니다.  
            // 랭킹 종료일
            "rankEndDateAndTime": {
                "S": "2021-03-15T05:00:00.000Z"
            },
            // 랭킹 기간
            // day : 일간
            // week : 주간
            // month : 월간
            // infinity : 누적 랭킹
            // custom : 일회성 랭킹
            "date": {
                "S": "custom"
            },
            // 랭킹 uuid
            "uuid": {
                "S": "90c25d70-8535-11eb-b01b-af47a424fd51"
            },
            // 랭킹 정렬 방법
            // desc : 내림차순
            // asc : 오름차순
            "order": {
                "S": "desc"
            },
            // 랭킹 초기화 시 랭킹에 사용한 컬럼을 초기화할 것인지 여부
            "isReset": {
                "BOOL": false
            },
            // 랭킹의 이름
            "title": {
                "S": "길드 랭킹"
            },
            // 콘솔에서 설정한 보상 우편 제목(fallback으로 출력)
            "rewardPostTitle": {
                "S": "우편 보상 제목"
            },
            // 랭킹에 사용한 굿즈 / 메타 구분
            // 굿즈 랭킹인 경우 "goods", 메타 랭킹인 경우 "meta"
            "table":{
                "S": "meta"
            },
            // 랭킹에 사용한 굿즈 / 메타 정보
            // 굿즈 랭킹인 경우 랭킹에 사용한 굿즈 종류에 따라 totalGoods1Amount ~ totalGoods10Amount
            // 메타 랭킹인 경우 랭킹에 사용한 메타의 Key
            "column":
            {
                "S": "totalDamage"
            }
        },
        // and etc...  
    ]
}
```

## Sample Code

```js
public class RankTableItem
{
    public string rankType;
    public string date;
    public string uuid;
    public string order;
    public bool isReset;
    public string title;
    public string table;
    public string column;

    //일회성 랭킹에만 존재
    public DateTime rankStartDateAndTime;
    public DateTime rankEndDateAndTime;

    //추가 항목이 있을 경우에만 존재
    public string extraDataColumn;
    public string extraDataType;

    public override string ToString()
    {
        string str = $"rankType:{rankType}\n" +
        $"date:{date}\n" +
        $"uuid:{uuid}\n" +
        $"order:{order}\n" +
        $"isReset:{isReset}\n" +
        $"title:{title}\n" +
        $"table:{table}\n" +
        $"column:{column}\n";

        if(rankStartDateAndTime != DateTime.MinValue)
        {
            str += $"rankStartDateAndTime:{rankStartDateAndTime}\n" +
        $"rankEndDateAndTime:{rankEndDateAndTime}\n";
        }

        if(extraDataColumn != null)
        {
            str += $"extraDataColumn:{extraDataColumn}\n" +
        $"extraDataType:{extraDataType}\n";
        }

        return str;
    }
}
```

```js
public void GetRankTableListTest()
{
    List<RankTableItem> rankTableItemList = new List<RankTableItem>();

    BackendReturnObject bro = Backend.URank.User.GetRankTableList();

    if(bro.IsSuccess())
    {
        LitJson.JsonData rankTableListJson = bro.FlattenRows();

        for(int i = 0; i < rankTableListJson.Count; i++)
        {
            RankTableItem rankTableItem = new RankTableItem();


            rankTableItem.rankType = rankTableListJson[i]["rankType"].ToString();
            rankTableItem.date = rankTableListJson[i]["date"].ToString();
            rankTableItem.uuid = rankTableListJson[i]["uuid"].ToString();
            rankTableItem.order = rankTableListJson[i]["order"].ToString();
            rankTableItem.isReset = rankTableListJson[i]["isReset"].ToString() == "true" ? true : false;
            rankTableItem.title = rankTableListJson[i]["title"].ToString();
            rankTableItem.table = rankTableListJson[i]["table"].ToString();
            rankTableItem.column = rankTableListJson[i]["column"].ToString();

            if(rankTableListJson[i].ContainsKey("rankStartDateAndTime"))
            {
                rankTableItem.rankStartDateAndTime = DateTime.Parse(rankTableListJson[i]["rankStartDateAndTime"].ToString());
                rankTableItem.rankEndDateAndTime = DateTime.Parse(rankTableListJson[i]["rankEndDateAndTime"].ToString());
            }

            if(rankTableListJson[i].ContainsKey("extraDataColumn"))
            {
                rankTableItem.extraDataColumn = rankTableListJson[i]["extraDataColumn"].ToString();
                rankTableItem.extraDataType = rankTableListJson[i]["extraDataType"].ToString();
            }

            rankTableItemList.Add(rankTableItem);
            Debug.Log(rankTableItem.ToString());
        }
    }
}
```
