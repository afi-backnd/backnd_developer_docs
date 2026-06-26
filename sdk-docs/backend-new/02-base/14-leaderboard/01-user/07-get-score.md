---
sidebar_label: 특정 점수를 가진 유저 랭킹 조회
description: "특정 점수를 가진 유저 랭킹 조회"
---

# GetRanksByScore
public Task&lt;GetUserRanksByScoreResult&gt; **GetRanksByScoreAsync**(string **leaderboardUuid**, int **score**);  
public Task&lt;GetUserRanksByScoreResult&gt; **GetRanksByScoreAsync**(string **leaderboardUuid**, long **score**);


## 파라미터
| Value        | Type           | Description  |  
| :------------ |:-------------| :----- | 
| leaderboardUuid | string | 조회할 랭킹의 uuid |  
| score | int/long | 조회할 점수 | 

leaderboardUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 유저 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/user/get-leaderboard) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹에 등록된 랭커들 중 score 값을 가지고 있는 랭커와 해당 점수 위, 아래 점수의 랭커를 조회합니다.  
* 조회한 score의 유저가 여러 명 존재할 경우 모든 유저가 조회됩니다.  
* 해당 score의 위, 아래 점수의 유저가 여러 명 존재하더라도 각각 단 한 명의 유저만 조회됩니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

### GetUserRanksByScoreResult
```js
public class UserRankInfo
{
    public string UserInDate;
    public string Nickname;
    public int Index;
    public int Rank;
    public string Score;
    public string ExtraName;
    public string ExtraData;
}

public sealed class GetUserRanksByScoreResult : RequestResult
{    
    public long GetTotalCount();
    public List<UserRankInfo> GetTargetRankList();
    public List<UserRankInfo> GetPrevRankList();
    public List<UserRankInfo> GetNextRankList();
}
```

## Example

### Task 방식
```js
var score = 0; //조회할 점수
var reqResult = await BackndLeaderboard.User.GetRanksByScoreAsync("01920445-a435-759e-bc3b-048a1e7a87aa", 5);

Debug.Log(reqResult.GetTotalCount());

// 검색한 스코어보다 더 높은 순위의 유저
foreach (var info in reqResult.GetNextRankList())
{
    Debug.Log($"{info.Rank}위 : {info.Nickname}");
    Debug.Log(info.ToString());
}

foreach (var info in reqResult.GetTargetRankList())
{
    Debug.Log($"{info.Rank}위 : {info.Nickname}");
    Debug.Log(info.ToString());
}

foreach (var info in reqResult.GetPrevRankList())
{
    Debug.Log($"{info.Rank}위 : {info.Nickname}");
    Debug.Log(info.ToString());
}
```

### Callback 방식
```js
var score = 0; //조회할 점수
BackndLeaderboard.User.GetRanksByScore("leaderboardUuid", score, reqResult =>
{
    Debug.Log(reqResult.GetTotalCount());

    // 검색한 스코어보다 더 높은 순위의 유저
    foreach (var info in reqResult.GetNextRankList())
    {
        Debug.Log($"{info.Rank}위 : {info.Nickname}");
        Debug.Log(info.ToString());
    }

    foreach (var info in reqResult.GetTargetRankList())
    {
        Debug.Log($"{info.Rank}위 : {info.Nickname}");
        Debug.Log(info.ToString());
    }

    // 검색한 스코어보다 더 낮은 순위의 유저
    foreach (var info in reqResult.GetPrevRankList())
    {
        Debug.Log($"{info.Rank}위 : {info.Nickname}");
        Debug.Log(info.ToString());
    }
});
```


## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**조회에 성공하였으나 랭킹의 유저가 등록되지 않은 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[],"previousRank":{"NULL":true},"nextRank":{"NULL":true},"totalCount":0}

**조회에 성공하였으나 해당 점수의 유저가 존재하지 않는 경우**  
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
### 해당 점수의 유저와 위, 아래 점수를 가진 유저가 모두 존재하는 경우
```js
{
    "rows": [
        {
            "gamerInDate": "2024-05-08T07:17:20.455Z",
            "nickname": "match5",
            "rank": 5,
            "index": 4,
            "score": "5"
        }
    ],
    "previousRank": [
        {
            "gamerInDate": "2024-05-08T07:17:20.928Z",
            "nickname": "match6",
            "rank": 4,
            "index": 3,
            "score": "6"
        }
    ],
    "nextRank": [
        {
            "gamerInDate": "2024-05-08T07:17:19.951Z",
            "nickname": "match4",
            "rank": 6,
            "index": 5,
            "score": "4"
        }
    ],
    "totalCount": 10
}
```

### 해당 점수를 가진 유저는 없지만, 해당 점수보다 높은 점수를 가진 유저는 존재하는 경우
```js
{
    "rows": [],
    "previousRank": [
        {
            "gamerInDate": "2024-05-08T07:17:17.625Z",
            "nickname": "match0",
            "rank": 10,
            "index": 9,
            "score": "0"
        }
    ],
    "nextRank": {
        "NULL": true
    },
    "totalCount": 10
}
```

### 해당 점수를 가진 유저는 없지만, 해당 점수보다 낮은 점수를 가진 유저는 존재하는 경우
```js
{
    "rows": [],
    "previousRank": {
        "NULL": true
    },
    "nextRank": [
        {
            "gamerInDate": "2024-05-08T07:17:22.518Z",
            "nickname": "match9",
            "rank": 1,
            "index": 0,
            "score": "9"
        }
    ],
    "totalCount": 10
}
```
