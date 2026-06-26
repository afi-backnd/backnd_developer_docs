---
sidebar_label: 특정 점수를 가진 길드 랭킹 조회
---

# GetRanksByScore
public Task&lt;GetGuildRanksByScoreResult&gt; **GetRanksByScoreAsync**(string **leaderboardUuid**, int **score**);  
public Task&lt;GetGuildRanksByScoreResult&gt; **GetRanksByScoreAsync**(string **leaderboardUuid**, long **score**);


## 파라미터
| Value        | Type           | Description  |  
| :------------ |:-------------| :----- | 
| leaderboardUuid | string | 조회할 랭킹의 uuid |  
| score | int/long | 조회할 점수 | 

leaderboardUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 길드 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/guild/get-leaderboard) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹에 등록된 랭커들 중 score 값을 가지고 있는 랭커와 해당 점수 위, 아래 점수의 랭커를 조회합니다.  
* 조회한 score의 길드가 여러 명 존재할 경우 모든 길드가 조회됩니다.  
* 해당 score의 위, 아래 점수의 길드가 여러 명 존재하더라도 각각 단 한 명의 길드만 조회됩니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

### GetGuildRanksByScoreResult
```js
public class GuildRankInfo
{
    public string GuildInDate;
    public string GuildName;
    public int Index;
    public int Rank;
    public string Score;
}

public sealed class GetGuildRanksByScoreResult : RequestResult
{    
    public long GetTotalCount();
    public List<GuildRankInfo> GetTargetRankList();
    public List<GuildRankInfo> GetPrevRankList();
    public List<GuildRankInfo> GetNextRankList();
}
```

## Example

### Task 방식
```js
int score = 0; //조회할 점수

GetGuildRanksByScoreResult reqResult;

reqResult = await BackndLeaderboard.Guild.GetRanksByScoreAsync("01920444-612d-7802-8e30-f983de5220a9", score);

Debug.Log("리더보드 총 길드 등록 수 : " + reqResult.GetTotalCount());

// 검색한 스코어보다 더 높은 순위의 길드
foreach (var info in reqResult.GetNextRankList())
{
    Debug.Log($"{info.Rank}위 : {info.GuildName}");
    Debug.Log(info.ToString());
}

foreach (var info in reqResult.GetTargetRankList())
{
    Debug.Log($"{info.Rank}위 : {info.GuildName}");
    Debug.Log(info.ToString());
}

// 검색한 스코어보다 더 낮은 순위의 길드   
foreach (var info in reqResult.GetPrevRankList())
{
    Debug.Log($"{info.Rank}위 : {info.GuildName}");
    Debug.Log(info.ToString());
}
```

### Callback 방식
```js
int score = 0; //조회할 점수
BackndLeaderboard.Guild.GetRanksByScore("leaderboardUuid", score, bro =>
{
    Debug.Log(bro.GetTotalCount());

    // 검색한 스코어보다 더 높은 순위의 길드
    foreach (var info in reqResult.GetNextRankList())
    {
        Debug.Log($"{info.Rank}위 : {info.GuildName}");
        Debug.Log(info.ToString());
    }

    foreach (var info in reqResult.GetTargetRankList())
    {
        Debug.Log($"{info.Rank}위 : {info.GuildName}");
        Debug.Log(info.ToString());
    }

    // 검색한 스코어보다 더 낮은 순위의 길드   
    foreach (var info in reqResult.GetPrevRankList())
    {
        Debug.Log($"{info.Rank}위 : {info.GuildName}");
        Debug.Log(info.ToString());
    }
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

**조회에 성공하였으나 랭킹의 길드가 등록되지 않은 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[],"previousRank":{"NULL":true},"nextRank":{"NULL":true},"totalCount":0}

**조회에 성공하였으나 해당 점수의 길드가 존재하지 않는 경우**  
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

## ReturnValueJson
### 해당 점수의 길드와 위, 아래 점수를 가진 길드가 모두 존재하는 경우
```js
{
    "rows": [
        {
            "guildName": "usa5G",
            "guildInDate": "2024-09-18T08:44:59.499Z",
            "rank": 24,
            "index": 23,
            "score": "5"
        },
        {
            "guildName": "japan5G",
            "guildInDate": "2024-09-18T08:42:09.000Z",
            "rank": 24,
            "index": 23,
            "score": "5"
        }
    ],
    "previousRank": [
        {
            "guildName": "japan3G",
            "guildInDate": "2024-09-12T05:04:37.350Z",
            "rank": 23,
            "index": 22,
            "score": "6"
        }
    ],
    "nextRank": [
        {
            "guildName": "usa4G",
            "guildInDate": "2024-09-18T08:44:53.785Z",
            "rank": 26,
            "index": 25,
            "score": "4"
        }
    ],
    "totalCount": 36
}
```

### 해당 점수를 가진 길드는 없지만, 해당 점수보다 높은 점수를 가진 길드는 존재하는 경우
```js
{
    "rows": [],
    "previousRank": [
        {
            "guildName": "japan1G",
            "guildInDate": "2024-09-18T08:41:46.845Z",
            "rank": 36,
            "index": 35,
            "score": "1"
        }
    ],
    "nextRank": {
        "NULL": true
    },
    "totalCount": 36
}
```

### 해당 점수를 가진 길드는 없지만, 해당 점수보다 낮은 점수를 가진 길드는 존재하는 경우
```js
{
    "rows": [],
    "previousRank": {
        "NULL": true
    },
    "nextRank": [
        {
            "guildName": "match9G",
            "guildInDate": "2024-05-08T07:17:22.915Z",
            "rank": 1,
            "index": 0,
            "score": "36"
        }
    ],
    "totalCount": 36
}
```
