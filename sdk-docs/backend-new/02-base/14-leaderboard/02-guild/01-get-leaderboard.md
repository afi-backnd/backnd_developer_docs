---
sidebar_label: 리더보드 불러오기
---

# GetLeaderboards

public Task&lt;GetLeaderboardsResult&gt; **GetLeaderboardsAsync**();  

## 설명

뒤끝 콘솔에 생성한 모든 유저 리더보드를 리스트 형태로 리턴합니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

해당 리스트에는 아래 정보가 포함되어 있습니다.  
* 리더보드 이름(title)
* 리더보드 uuid(uuid)
* 리더보드 생성 날짜(inDate)
* 리더보드 구분(rankType)
* 리더보드 주기(date)
* 초기화 시간(initializationTime)
* 그룹 구분 여부(isDivision)
* 우편 보상 이름(rewardPostTitle)
* 리더보드 초기화 시 데이터 초기화 여부(isReset)
* 리더보드 정렬 방법(order)
* 리더보드에 사용한 테이블(table)
* 리더보드에 사용한 컬럼(column)
* 리더보드에 사용한 추가 항목(extraDataColumn)
* 리더보드에 사용한 추가 항목의 데이터타입(extraDataType)


### GetLeaderboardsResult
```js
public sealed class GetLeaderboardsResult : ListInfoResult<LeaderboardInfo>
{
    public List<LeaderboardInfo> GetInfoList();
}
```

### LeaderboardInfo
```js
public sealed class LeaderboardInfo
{
    public string RankType;
    public string Date;
    public string Uuid;
    public string Order;
    public string Title;
    public string Table;
    public string Column;
    public string InitializationTime;
    public bool IsReset;
    public bool IsDivision;

    // 일회성 랭킹 전용.
    public string RankStartDateAndTime { get; private set; }
    public string RankEndDateAndTime { get; private set; }

    // ExtraData가 있을 경우에만 존재.
    public string ExtraDataColumn { get; private set; }
    public string ExtraDataType { get; private set; }

    public Dictionary<string, string> RewardPostTitle { get; private set; }
}
```

## Example

### Task 형식
```js
var reqResult = await BackndLeaderboard.Guild.GetLeaderboardsAsync();
foreach (var info in reqResult.GetInfoList())
{
    Debug.Log(info.ToString());
}
```

### Callback 형식
```js
BackndLeaderboard.Guild.GetLeaderboards((callback) =>
{
    foreach (var info in callback.GetInfoList())
    {
        Debug.Log(info.ToString());
    }
});
```

:::tip `LeaderboardInfo` 사용 시 알림
initializationTime 같이 시간을 나타내는 변수는 string 타입으로 선언 되어 전달됩니다.  
사용 하시려는 목적에 맞게 전달 된 값을 파싱하여 사용하시기 바랍니다.
:::
:::note 값이 공백으로 나오는 경우  
가져오려는 필드의 값이 없는 경우 공백으로 나올 수 있습니다.  
ReturnValueJson 을 참고하여 값이 존재하는지 확인 가능합니다.  
:::

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson  참조


### Error cases

**리더보드가 없는 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다

## ReturnValueJson 
```js
{
    "rows": [
        {
            "rankType": "guild", // 리더보드 타입(user / guild)
            "isDivision": true, // 그룹 구분 여부
            "rewardInDate": "2024-07-30T04:52:13.448Z",
            "order": "desc", // 정렬 순서
            "initializationTime": "04:00:00 UTC+9:00", // 리더보드 초기화 시간
            "rankStartDateAndTime": "2024-08-19T15:40:00.000Z", // (일회성, 기한없음 리더보드에서만) 리더보드 시작 시간
            "rankEndDateAndTime": "2024-08-20T15:00:00.000Z", // (일회성 리더보드에서만) 리더보드 종료 시간
            // 랭킹 기간
            // day : 일간
            // week : 주간
            // month : 월간
            // infinity : 누적 랭킹
            // custom : 일회성 랭킹
            "date": "custom", // 
            "inDate": "2024-08-20T07:08:09.478Z", // 리더보드 생성 일시
            "uuid": "01916e9d-5186-7594-b8ab-e286cefb6226",
            "isReset": true, // 초기화 시 데이터 리셋 여부
            // 언어별 보상 이름
            "rewardPostTitle": {
                "ko": "한국보상",
                "en": "EnglishReward",
                "fallback": "en"
            },
            "rewardName": "보상 차트 이름",
            "title": "랭킹 제목",
            "table": "meta", // meta / goods
            "column": "랭킹에 사용된 컬럼 이름" // meta에 설정된 컬럼명 or totalGoods1Amount ~ totalGoods10Amount
        },
        ...
    ]
}
```
