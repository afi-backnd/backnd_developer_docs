---
sidebar_label: "유저 리더보드 과거 전체 순위 확인"
---

# GetPastRanks
public Task&lt;UserRankResult&gt; **GetPastRanksAsync**(string **leaderboardUuid**, int **limit = 10**, int **offset = 0**)

## 파라미터
| Value        | Type           | Description  | Default |  
| :------------ |:-------------| :----- | :----- |
| leaderboardUuid | string | 조회할 리더보드의 uuid | -  |
| limit | int | 불러올 리더보드 유저의 수 (1 ~ 50) | 10 |  
| offset | int | 조회할 리더보드의 uuid(0이상) | 0 |



## 설명
리더보드 보상이 측정된 리더보드의 갱신되기 전 마지막 리더보드 순위를 불러옵니다.  
* 보상이 존재하는 리더보드만 가능합니다.  
* 리더보드 보상을 받은 유저들의 과거 리더보드만 표시됩니다.
  * 리더보드 보상이 10등까지에게만 주어질 경우, 11등부터의 과거 리더보드은 불러올 수 없습니다.
  * 순위권 밖이지만 전체 보상의 범위에 포함된 유저도 과거 리더보드에는 표시되지 않습니다.
* 특정 유저의 과거 리더보드, 내 과거 리더보드만 특정하여 불러올 수 없습니다.  
* 과거 리더보드 중 제일 최신인 과거 리더보드 하나를 대상으로 불러옵니다.
  * 일간 리더보드일 경우, 1일 전 과거 리더보드 데이터를 불러오게 되며 2일 전 리더보드은 불러오지 못합니다.
* limit을 0이하로 입력할 경우 1로 적용됩니다. 50이상일 경우에는 50으로 적용됩니다.
* offset을 0이하로 입력할 경우 0으로 적용됩니다.
* 닉네임이 존재하지 않는 유저의 경우, ""으로 출력됩니다.
* 해당 함수는 SendQueue로 호출할 수 없습니다.

:::danger 탈퇴 유저의 경우
과거 리더보드 리스트중 탈퇴한 유저가 있을 경우에는 해당 유저의 nickname과 inDate 컬럼은 존재하지 않습니다.  
jsonData.ContainsKey("nickname")으로 컬럼을 확인한 뒤에 데이터를 가공해주세요.

탈퇴된 유저
```
{"score":"7835","rank":1}
```

일반 유저
```
{"score":"7833","rank":3,"nickname":"user1","inDate":"2024-03-06T05:59:53.523Z"}
```
:::

:::caution 초기화 날짜를 지정하여 만든 리더보드의 과거 전체 순위 확인
초기화 날짜가 지정된 1회성 리더보드의 경우 지정한 초기화 날짜 이전에 `GetPastRanks` 또는 `GetPastRankList`를 호출하여 전체 순위 데이터를 요청하게 되면 에러값을 리턴합니다.  
이 에러는 함수를 호출한 시점의 시간 보다 리더보드 초기화 날짜의 값이 더 크기 때문에 발생하는 에러입니다.  
리더보드의 초기화 날짜 이후에 함수를 호출하면 정상 값을 리턴합니다.  
:::

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
// 리더보드 1위 부터 10위까지
var reqResult = await BackndLeaderboard.User.GetPastRanksAsync("01920445-a435-759e-bc3b-048a1e7a87aa");
foreach (var info in reqResult.GetInfoList())
{
    string name = $"[{info.Rank}] {info.Nickname} : {info.Score}";
    Debug.Log(info);
}

// 리더보드 11위(offset + 1) 부터 61위까지((offset + 1 + limit)
reqResult = await BackndLeaderboard.User.GetPastRanksAsync("a61c6980-db5f-11ee-95a9-73cbb1225013", 50, 10);
```

### Callback 방식
```js
BackndLeaderboard.User.GetPastRanks("a61c6980-db5f-11ee-95a9-73cbb1225013", 10, 0, callback =>
{
    foreach (var info in reqResult.GetInfoList())
    {
        string name = $"[{info.Rank}] {info.Nickname} : {info.Score}";
        Debug.Log(info);
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

**과거 리더보드의 보상 집계가 되지 않은 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}  

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : leaderboardUuid is null or empty

**초기화 날짜를 지정하여 만든 리더보드의 종료일 이전에 과거 전체 순위 확인을 요청 한 경우**  
statusCode : 400  
errorCode : ValidationException  
message : The BETWEEN condition was provided a range where the lower bound is greater than the upper bound

**존재하지 않는 uuid로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다

**누적 리더보드의 과거 순위를 조회 시도한 경우**  
statusCode : 428  
errorCode : Precondition Required  
message : Precondition Required rank initial type  

## ReturnValueJson
```js
{
    "rows": [
        {
            "score": "7835", // 탈퇴된 유저일 경우 nickname과 inDate가 출력되지 않습니다.
            "rank": 1
        }
        {
            "score": "5834",
            "rank": 2,
            "nickname": "user2",// 닉네임이 존재하지 않는 유저일 경우, ""로 출력됩니다
            "inDate": "2024-03-06T05:59:53.523Z"
        },
        {
            "score": "5121",
            "rank": 3,
            "nickname": "user3",
            "inDate": "2024-03-06T05:59:51.568Z"
        }
    ]
}
```
