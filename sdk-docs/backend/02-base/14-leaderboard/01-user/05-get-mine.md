---
sidebar_label: "내 리더보드 순위 조회"
description: "GetMyLeaderboard"
---

# GetMyLeaderboard
public BackendUserLeaderboardReturnObject **GetMyLeaderboard**(string **leaderboardUuid**);  
public BackendUserLeaderboardReturnObject **GetMyLeaderboard**(string **leaderboardUuid**, int **gap**);


## 파라미터
| Value        | Type           | Description  | Default | 
| :------------ |:-------------| :----- | :--- |
| leaderboardUuid | string | 조회할 랭킹의 uuid | - | 
| gap | int | 함께 조회할 위아래 랭커의 수(0 ~ 25) | 0 |

leaderboardUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 유저 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/user/get-leaderboard) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹에서 자신의 랭킹을 조회합니다.  
* 동점(랭킹이 같은) 유저가 존재할 경우 gap 함수를 통해 조회 시 동일한 랭킹의 유저가 리턴될 수 있습니다. 
* 해당 함수는 SendQueue로 호출할 수 없습니다.

### BackendUserLeaderboardReturnObject
```js
namespace BackEnd.Leaderboard
{
    public class UserLeaderboardItem
    {
        public string gamerInDate;
        public string nickname = string.Empty;
        public string score;
        public string index;
        public string rank;
        public string extraData = string.Empty;
        public string extraName = string.Empty;
    }

    public class BackendUserLeaderboardReturnObject : BackendReturnObject
    {    
        public long GetTotalCount();
        public List<UserLeaderboardItem> GetUserLeaderboardList();
    }
}
```

## Example

### 동기
```js
BackEnd.Leaderboard.BackendUserLeaderboardReturnObject bro = null;
// 자기 자신의 랭킹만 조회
bro = Backend.Leaderboard.User.GetMyLeaderboard("leaderboardUuid");
// 위아래 3명의 유저를 포함하여 랭킹 조회
// 자신이 4등인 경우 1 ~ 7등 유저가 조회됨
bro = Backend.Leaderboard.User.GetMyLeaderboard("leaderboardUuid", 3);

if (bro.IsSuccess() == false)
{
    return;
}

Debug.Log("리더보드 총 유저 등록 수 : " + bro.GetTotalCount());

foreach (BackEnd.Leaderboard.UserLeaderboardItem item in bro.GetUserLeaderboardList())
{
    Debug.Log($"{item.rank}위({item.score}) : {item.nickname}");
    Debug.Log(item.ToString());
}
```

### 비동기
```js
// 자기 자신의 랭킹만 조회
Backend.Leaderboard.User.GetMyLeaderboard("leaderboardUuid", bro => {
    if(bro.IsSuccess() == false) {
        return;
    }

    Debug.Log("리더보드 총 유저 등록 수 : " + bro.GetTotalCount());

    foreach (BackEnd.Leaderboard.UserLeaderboardItem item in bro.GetUserLeaderboardList())
    {
        Debug.Log($"{item.rank}위({item.score}) : {item.nickname}");
        Debug.Log(item.ToString());
    }
});

// 위아래 3명의 유저를 포함하여 랭킹 조회
// 자신이 4등인 경우 1 ~ 9등 유저가 조회됨
Backend.Leaderboard.User.GetMyLeaderboard("leaderboardUuid", 5, bro => {
    // 이후 처리
});
```


## ReturnCase

### Success cases

**자신이 랭킹에 존재하고, 조회에 성공한 경우**  
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

**자신의 랭킹이 존재하지 않는 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : userRank not found, userRank을(를) 찾을 수 없습니다

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

### gap이 1 인 경우(user4 기준)
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
