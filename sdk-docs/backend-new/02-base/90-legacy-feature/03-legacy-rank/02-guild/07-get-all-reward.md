---
sidebar_label: 길드 랭킹 보상 정보 조회
description: "길드 랭킹 보상 정보 조회"
---

# GetRankRewardList
public Task&lt;RequestResult&gt; **GetRankRewardListAsync**(string **rankUuid**);

:::info 기능 개선 안내
기존 랭킹의 성능과 기능을 개선한 **리더보드** 기능을 제공하고 있습니다.  
**콘솔에서 신규 생성은 리더보드만 제공**하므로 해당 기능을 사용해 주세요.
:::

:::warning 그룹 구분된 리더보드 사용 불가
URank 랭킹 함수는 그룹이 구분된 리더보드에서 NULL그룹만 조회할 수 있습니다.  
그룹에 따른 리더보드를 확인하고자 할 경우에는 Leaderboard함수를 이용해주세요. 
:::

## 파라미터
| Value        | Type           | Description  |  
| :------------ |:-------------| :----- | 
| rankUuid | string | 조회할 랭킹의 uuid |  

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 길드 랭킹 정보 조회](/sdk-docs/backend/base/rank/guild/get-all-settings) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹의 보상 정보를 조회합니다.  
랭킹 보상을 수령하는 것이 아닌 콘솔에서 등록한 랭킹의 보상 정보를 조회하는 함수입니다.  

랭킹 보상을 수령하는 방법에 대해서는 [우편 조회](/sdk-docs/backend/base/post/difference-to-old#구버전-코드post) 및 [우편 수령](/sdk-docs/backend/base/post/difference-to-old#구버전-코드post) 문서를 확인해 주세요.  

## Example

### Task 방식
```js
var reqResult = await BackndLegacy.URank.Guild.GetRankRewardListAsync("rankUuid");
```

### Callback 방식
```js
BackndLegacy.URank.Guild.GetRankRewardList("rankUuid", callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

### Error cases

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**존재하지 않는 uuid로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

**보상이 존재하지 않는 uuid로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank reward not found, rank reward을(를) 찾을 수 없습니다

## ReturnValueJson
```js
{
    "rows": [
        {
            // 랭킹 보상 아이템 정보
            "rewardItems": {
                // 아이템 정보
            },
            // 해당 랭킹 보상을 받을 시작 랭킹
            "startRank": "1",
            // 해당 랭킹 보상을 받을 마지막 랭킹
            "endRank": "1",
            // 수여할 아이템 개수
            "rewardItemCount": "1",
            // 아이템 정보가 포함된 차트 이름
            "rewardChartName": "weaponTable"
        },
        // and etc...  
    ]
}
```

## Sample Code
```js
public class RankRewardItem
{
    public int startRank;
    public int endRank;
    public int rewardItemCount;
    public string rewardChartName;
    public Dictionary<string, string> rewardItems = new Dictionary<string, string>();
    public override string ToString()
    {
        string str = string.Empty;
        foreach(var dic in rewardItems)
        {
            str += $"보상 : {dic.Key} : {dic.Value}\n";
        }
        return $"startRank : {startRank}\n" +
        $"endRank : {endRank}\n" +
        $"rewardItemCount : {rewardItemCount}\n" +
        $"rewardChartName : {rewardChartName}\n" + str;
    }
}
```

```js
public async Task GetRankRewardList()
{
    var reqResult = await BackndLegacy.URank.User.GetRankRewardListAsync("9b41d1c0-8ef0-11ec-b651-c7dd98f693dc");
    if (!reqResult.IsSuccess())
    {
        Debug.LogError(reqResult.ToString());
        return;
    }

    List<RankRewardItem> rankRewardList = new List<RankRewardItem>();

    var rankRewardListJson = reqResult.GetRows();
    for (int i = 0; i < rankRewardListJson.Count; i++)
    {
        RankRewardItem rankRewardItem = new RankRewardItem();

        rankRewardItem.rewardItemCount = int.Parse(rankRewardListJson[i]["rewardItemCount"].ToString());
        rankRewardItem.startRank = int.Parse(rankRewardListJson[i]["startRank"].ToString());
        rankRewardItem.endRank = int.Parse(rankRewardListJson[i]["endRank"].ToString());
        rankRewardItem.rewardChartName = rankRewardListJson[i]["rewardChartName"].ToString();

        var items = rankRewardListJson[i]["rewardItems"] as JObject;
        foreach (var property in items.Properties())
        {
            var column = property.Name;
            rankRewardItem.rewardItems.Add(column, property.Value.ToString());
        }

        rankRewardList.Add(rankRewardItem);
        Debug.Log(rankRewardItem.ToString());
    }
}
```
