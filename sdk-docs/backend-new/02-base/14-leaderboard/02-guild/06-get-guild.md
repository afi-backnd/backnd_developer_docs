---
sidebar_label: 특정 길드 리더보드 순위 조회
description: "특정 길드 리더보드 순위 조회"
---

# GetOtherGuildRank
public Task&lt;GuildRankResult&gt; **GetOtherGuildRankAsync**(string **leaderboardUuid**, string **userInDate**);  
public Task&lt;GuildRankResult&gt; **GetOtherGuildRankAsync**(string **leaderboardUuid**, string **userInDate**, int **gap**);

## 파라미터
| Value        | Type           | Description  | Default | 
| :------------ |:-------------| :----- | :--- |
| leaderboardUuid | string | 조회할 리더보드의 uuid | - | 
| userIndate | string | 조회할 길드의 inDate | - |
| gap | int | 함께 조회할 위아래 랭커의 수(0 ~ 25) | 0 |

leaderboardUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 리더보드를 생성 후 해당 리더보드 정보에서 uuid 값 확인
* [모든 길드 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/guild/get-leaderboard) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값과 userIndate를 이용하여 해당 리더보드에서 해당 길드의 순위를 조회합니다.  
* 동점(랭킹이 같은) 길드가 존재할 경우 gap 함수를 통해 조회 시 동일한 랭킹의 길드가 리턴될 수 있습니다. 
* 해당 함수는 SendQueue로 호출할 수 없습니다.

### GuildRankResult
```js
public class GuildRankInfo
{
    public string GuildInDate;
    public string GuildName;
    public int Index;
    public int Rank;
    public string Score;
}

public sealed class GuildRankResult : ListInfoResult<GuildRankInfo>
{    
    public long GetTotalCount();
    public List<GuildRankResult> GetInfoList();
}
```

## Example

### Task 방식
```js
GuildRankResult reqResult;

// 자기 자신의 랭킹만 조회
reqResult = await BackndLeaderboard.Guild.GetOtherGuildRankAsync("01920444-612d-7802-8e30-f983de5220a9", "2024-05-08T07:17:20.830Z");
// 위아래 3명의 길드를 포함하여 리더보드 조회
// 해당 길드가 4등인 경우 1 ~ 7등 길드가 조회됨
reqResult = await BackndLeaderboard.Guild.GetOtherGuildRankAsync("01920444-612d-7802-8e30-f983de5220a9", "2024-05-08T07:17:20.830Z", 3);

Debug.Log("리더보드 총 길드 등록 수 : " + reqResult.GetTotalCount());
foreach (var info in reqResult.GetInfoList())
{
    Debug.Log($"{info.Rank}위 ({info.Score}) : {info.GuildName}");
    Debug.Log(info.ToString());
}
```

### Callback 방식
```js
// 해당 길드의 리더보드만 조회
BackndLeaderboard.Guild.GetOtherGuildRank("01920444-612d-7802-8e30-f983de5220a9", "2024-05-08T07:17:20.830Z", bro => {

    if (bro.IsSuccess() == false)
    {
        return;
    }

    Debug.Log("리더보드 총 길드 등록 수 : " + bro.GetTotalCount());

    foreach (var info in reqResult.GetInfoList())
    {
        Debug.Log($"{info.Rank}위 ({info.Score}) : {info.GuildName}");
        Debug.Log(info.ToString());
    }
});

// 위아래 3명의 길드를 포함하여 리더보드 조회
// 해당 길드가 4등인 경우 1 ~ 9등 길드가 조회됨
BackndLeaderboard.Guild.GetOtherGuildRank("01920444-612d-7802-8e30-f983de5220a9", "2024-05-08T07:17:20.830Z", 5, callback =>
{
    // 이후 처리
});
```


## ReturnCase

### Success cases

**해당 길드가 리더보드에 존재하는 경우**  
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

**존재하지 않는 guildIndate로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다

**해당 길드가 랭킹에 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guildRank not found, guildRank을(를) 찾을 수 없습니다

## ReturnValueJson

### gap이 0인 경우
```js
{
    "rows": [
        {
            "guildName": "내길드",
            "guildInDate": "2024-05-08T07:17:20.830Z",
            "rank": 5,
            "index": 4,
            "score": "20"
        }
    ],
    "totalCount": 36
}
```

### gap이 1 인 경우(내길드(2024-05-08T07:17:20.830Z) 기준)
```js
{
    "rows": [
        {
            "guildName": "guild3",
            "guildInDate": "2024-05-08T07:17:21.347Z",
            "rank": 4,
            "index": 3,
            "score": "24"
        },
        {
            "guildName": "내길드",
            "guildInDate": "2024-05-08T07:17:20.830Z",
            "rank": 5,
            "index": 4,
            "score": "20"
        },
        {
            "guildName": "guild5",
            "guildInDate": "2024-09-12T05:04:53.047Z",
            "rank": 6,
            "index": 5,
            "score": "18"
        }
    ],
    "totalCount": 36
}
```

