---
sidebar_label: 특정 유저 리더보드 순위 조회
description: "특정 유저 리더보드 순위 조회"
---

# GetOtherUserRank
public Task&lt;UserRankResult&gt; **GetOtherUserRankAsync**(string **leaderboardUuid**, string **userInDate**);  
public Task&lt;UserRankResult&gt; **GetOtherUserRankAsync**(string **leaderboardUuid**, string **userInDate**, int **gap**);

## 파라미터
| Value        | Type           | Description  | Default | 
| :------------ |:-------------| :----- | :--- |
| leaderboardUuid | string | 조회할 리더보드의 uuid | - | 
| userIndate | string | 조회할 유저의 inDate | - |
| gap | int | 함께 조회할 위아래 랭커의 수(0 ~ 25) | 0 |

leaderboardUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 리더보드를 생성 후 해당 리더보드 정보에서 uuid 값 확인
* [모든 유저 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/user/get-leaderboard) 함수를 이용하여 uuid 값 확인

userIndate 값은 아래 방법을 통해 확인할 수 있습니다.  
* 친구의 inDate 확인
* 길드원의 inDate 확인
* public 테이블의 owner_inDate 컬럼을 통해 확인
* 기타 유저의 inDate를 공유하는 방법 등을 통해 확인

## 설명
uuid 값과 userIndate를 이용하여 해당 리더보드에서 해당 유저의 순위를 조회합니다.  
* 동점(랭킹이 같은) 유저가 존재할 경우 gap 함수를 통해 조회 시 동일한 랭킹의 유저가 리턴될 수 있습니다. 
* 해당 함수는 SendQueue로 호출할 수 없습니다.

### UserRankResult
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

public sealed class UserRankResult : ListInfoResult<UserRankInfo>
{    
    public long GetTotalCount();
    public List<UserRankInfo> GetInfoList();
}
```

## Example

### Task 방식
```js
UserRankResult ranksResult = null;

// 해당 유저의 리더보드만 조회
ranksResult = await BackndLeaderboard.User.GetOtherUserRankAsync("leaderboardUuid", "userIndate");

// 위아래 3명의 유저를 포함하여 리더보드 조회
// 해당 유저가 4등인 경우 1 ~ 7등 유저가 조회됨
ranksResult = await BackndLeaderboard.User.GetOtherUserRankAsync("leaderboardUuid", "userIndate", 3);

if (ranksResult.IsSuccess() == false)
{
    return;
}

Debug.Log("리더보드 총 유저 등록 수 : " + ranksResult.GetTotalCount());

foreach (var info in ranksResult.GetInfoList())
{
    Debug.Log($"{info.Rank}위 : {info.Nickname}");
    Debug.Log(info.ToString());
}
```

### Callback 방식
```js
BackndLeaderboard.User.GetOtherUserRank("leaderboardUuid", "userIndate", callback =>
{
    if (callback.IsSuccess() == false)
    {
        return;
    }

    Debug.Log("리더보드 총 유저 등록 수 : " + callback.GetTotalCount());

    foreach (var info in ranksResult.GetInfoList())
    {
        Debug.Log($"{info.Rank}위 : {info.Nickname}");
        Debug.Log(info.ToString());
    }
});

// 위아래 3명의 유저를 포함하여 리더보드 조회
// 해당 유저가 4등인 경우 1 ~ 9등 유저가 조회됨
BackndLeaderboard.User.GetOtherUserRank("leaderboardUuid", "userIndate", 5, callback =>
{
    // 이후 처리
});
```


## ReturnCase

### Success cases

**해당 유저가 리더보드에 존재하는 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : leaderboardUuid is null or empty

**존재하지 않는 uuid로 조회를 시도한 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다

**존재하지 않는 유저의 userIndate로 조회를 시도할 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer not found, gamer을(를) 찾을 수 없습니다

**해당 유저가 리더보드에 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : userRank not found, userRank을(를) 찾을 수 없습니다

## GetReturnValuetoJSON
### gap이 0인 경우
```js
{
    "rows": [
        {
            "gamerInDate": "2024-05-08T07:17:20.455Z",
            "nickname": "user4",
            "rank": 5,
            "index": 4,
            "score": "5"
        }
    ],
    "totalCount": 10
}
```

### gap이 1 인 경우(2024-05-08T07:17:20.455Z 기준)
```js
{
    "rows": [
        {
            "gamerInDate": "2024-05-08T07:17:20.928Z",
            "nickname": "user5",
            "rank": 4,
            "index": 3,
            "score": "6"
        },
        {
            "gamerInDate": "2024-05-08T07:17:20.455Z",
            "nickname": "user4",
            "rank": 5,
            "index": 4,
            "score": "5"
        },
        {
            "gamerInDate": "2024-05-08T07:17:19.951Z",
            "nickname": "user3",
            "rank": 6,
            "index": 5,
            "score": "4"
        }
    ],
    "totalCount": 10
}
```
