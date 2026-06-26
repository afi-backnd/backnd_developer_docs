---
sidebar_label: 리더보드 보상 정보 조회
---

# GetRankRewards
public Task&lt;GetRankRewardsResult&gt; **GetRankRewardsAsync**(string **leaderboardUuid**);


## 파라미터
| Value        | Type           | Description  |  
| :------------ |:-------------| :----- | 
| leaderboardUuid | string | 조회할 랭킹의 uuid |  

leaderboardUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 유저 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/user/get-leaderboard) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹의 보상 정보를 조회합니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.


랭킹 보상을 수령하는 것이 아닌 콘솔에서 등록한 랭킹의 보상 정보를 조회하는 함수입니다.  

랭킹 보상을 수령하는 방법에 대해서는 [우편 조회](/sdk-docs/backend/base/post/difference-to-old#구버전-코드post) 및 [우편 수령](/sdk-docs/backend/base/post/difference-to-old#구버전-코드post) 문서를 확인해 주세요.  

### GetRankRewardsResult
```js
public class RankRewardInfo
{
    public int StartRank;
    public int EndRank;
    public int ItemCount;
    public string ChartName;
    public Dictionary<string, string> ItemFields
        = new Dictionary<string, string>();
}

public sealed class GetRankRewardsResult : ListInfoResult<RankRewardInfo>
{    
    public List<RankRewardInfo> GetInfoList();
}
```


## Example

### Task 방식
```js
BackEnd.Leaderboard.BackendLeaderboardRewardReturnObject bro = Backend.Leaderboard.User.GetLeaderboardReward("uuid");

foreach(BackEnd.Leaderboard.LeaderboardRewardItem item in bro.GetLeaderboardRewardList())
{
    Debug.Log(item);
    if(item.rewardItems.ContainsKey("level"))
    {
        int level = int.Parse(item.rewardItems["level"]);
    }
}
```

### Callback 방식
```js
BackndLeaderboard.User.GetRankRewards("leaderboardUuid", reqResult =>
{
    foreach (var info in reqResult.GetInfoList())
    {
        Debug.Log(info);
        if (info.ItemFields.ContainsKey("level"))
        {
            int level = int.Parse(info.ItemFields["level"]);
        }
    }
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
message : leaderboardUuid is null or empty

**존재하지 않는 uuid로 조회를 시도한 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다

**보상이 존재하지 않는 uuid로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank reward not found, rank reward을(를) 찾을 수 없습니다

## ReturnValueJson
```js
{
    "rows": [
        {
            "rewardItems": {
                "chartFileName": "LevelChart.csv",
                "maxExperience": "100",
                "level": "1",
                "rewardGold": "1111"
            },
            "startRank": 0,
            "endRank": 0,
            "rewardItemCount": 123,
            "rewardChartName": "LevelChart"
        }
    ]
}
```
