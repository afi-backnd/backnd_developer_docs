---
sidebar_label: 리더보드 내 전체 순위 조회
---

# GetLeaderboard
public BackendGuildLeaderboardReturnObject **GetLeaderboard**(string **leaderboardUuid**);  
public BackendGuildLeaderboardReturnObject **GetLeaderboard**(string **leaderboardUuid**, int **limit**);  
public BackendGuildLeaderboardReturnObject **GetLeaderboard**(string **leaderboardUuid**, int **limit**, int **offset**);


## 파라미터
| Value        | Type           | Description  | Default | 
| :------------ |:-------------| :----- | :--- |
| leaderboardUuid | string | 조회할 랭킹의 uuid | - | 
| limit | int | 조회할 랭커의 수(1 ~ 50) | 10 |
| offset | int | 조회할 랭킹의 시작점(0 이상) | 0 |

leaderboardUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 길드 리더보드 정보 조회](/sdk-docs/backend/base/leaderboard/guild/get-leaderboard) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 **자신이 속한 그룹**의 랭커들의 리스트를 조회합니다.  
limit와 offset을 이용해 특정 위치의 랭커들을 조회할 수 있습니다.  
이를 사용하면 한 번에 최대 50명씩, 1위부터 마지막 랭커까지 조회가 가능합니다.  
* 그룹에 속해 있지 않는 유저의 경우, NULL그룹의 리더보드를 조회합니다.
> 공동 순위의 경우, 랭킹 정렬 기준(오름차순, 내림차순)과 같은 기준으로 gamer_id(회원번호)를 사전식 순서로 정렬합니다.  
* limit를 0이하로 입력할 경우 0으로 적용됩니다.  
* offset을 0이하로 입력할 경우 0으로 적용됩니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

### BackendGuildLeaderboardReturnObject
```js
namespace BackEnd.Leaderboard
{
    public class GuildLeaderboardItem
    {
        public string guildInDate;
        public string guildName;
        public string score;
        public string index;
        public string rank;
    }

    public class BackendGuildLeaderboardReturnObject : BackendReturnObject
    {    
        public long GetTotalCount();
        public List<GuildLeaderboardItem> GetGuildLeaderboardList();
    }
}
```

## Example

### 동기
```js
// example 1. 1위부터 150위까지 조회.
// leaderboardUuid 랭킹에서 1 ~ 50등 랭커 조회
bro = Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", 50);
// leaderboardUuid 랭킹에서 51 ~ 100등 랭커 조회
bro = Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", 50, 50);
// leaderboardUuid 랭킹에서 101 ~ 150등 랭커 조회
bro = Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", 50, 100);

// example 2. 특정 위치 조회 및 기본값 조회
// leaderboardUuid 랭킹에서 11등 ~ 30등 랭커 조회
bro = Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", 20, 10);
// leaderboardUuid 랭킹에서 1 ~ 10등 랭커 조회
bro = Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid");

// 랭킹 조회 후.
if (bro.IsSuccess() == false)
{
    return;
}

Debug.Log("리더보드 총 길드 등록 수 : " + bro.GetTotalCount());

foreach (BackEnd.Leaderboard.GuildLeaderboardItem item in bro.GetGuildLeaderboardList())
{
    Debug.Log($"{item.rank}위 : {item.guildName}");
    Debug.Log(item.ToString());
}
```

### 비동기
```js
// example 1. 1위부터 150위까지 조회.
// leaderboardUuid 랭킹에서 1 ~ 50등 랭커 조회
Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", 50, callback=> {
    if(callback.IsSuccess() == false) {
        return;
    }    

    Debug.Log("리더보드 총 길드 등록 수 : " + bro.GetTotalCount());
    
    foreach (BackEnd.Leaderboard.GuildLeaderboardItem item in bro.GetGuildLeaderboardList())
    {
        Debug.Log($"{item.rank}위 : {item.guildName}");
        Debug.Log(item.ToString());
    }
});
// leaderboardUuid 랭킹에서 51 ~ 100등 랭커 조회
Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", 50, 50 callback=> {
    // 이후 처리
});
// leaderboardUuid 랭킹에서 101 ~ 150등 랭커 조회
Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", 50, 100 callback=> {
    // 이후 처리
});

// example 2. 특정 위치 조회 및 기본값 조회
// leaderboardUuid 랭킹에서 11등 ~ 30등 랭커 조회
Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", 20, 10, callback=> {
    // 이후 처리
});
// leaderboardUuid 랭킹에서 1 ~ 10등 랭커 조회
Backend.Leaderboard.Guild.GetLeaderboard("leaderboardUuid", callback=> {
    // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공하였을 때**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**조회에 성공하였으나 조회를 시도한 범위에 랭커가 존재하지 않을 때**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

### Error cases

**leaderboardUuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : leaderboardUuid is null or empty

**존재하지 않는 uuid 일 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다

## GetReturnValuetoJSON
```js
{
    "rows": [
        {
            "guildName": "guild1",
            "guildInDate": "2024-05-08T07:17:22.915Z",
            "rank": 1,
            "index": 0,
            "score": "36"
        },
        {
            "guildName": "guild2",
            "guildInDate": "2024-05-08T07:17:22.408Z",
            "rank": 2,
            "index": 1,
            "score": "32"
        }
    ],
    "totalCount": 18
}
```
