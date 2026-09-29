---
sidebar_label: "리더보드 불러오기"
description: "GetLeaderboards"
---

# GetLeaderboards

public **BackendLeaderboardTableReturnObject** **GetLeaderboards**();  

## 설명

뒤끝 콘솔에 생성한 모든 길드 리더보드를 리스트 형태로 리턴합니다.  
* 해당 함수는 SendQueue로 호출할 수 없습니다.

조회 결과의 JSON 응답에는 아래 정보가 포함되어 있습니다.  
* 리더보드 이름(title)
* 리더보드 uuid(uuid)
* 리더보드 생성 날짜(inDate)
* 리더보드 구분(rankType)
* 리더보드 주기(date)
* 초기화 시간(initializationTime)
* 초기화 요일(initializationDay)
* 초기화 일자(initializationDate)
* 시작·종료 날짜 정보(rankStartDateAndTime, rankEndDateAndTime, 주기에 따라 제공 여부와 의미가 다름)
* 그룹 구분 여부(isDivision)
* 데이터베이스 테이블 사용 여부(isDatabase)
* 언어별 보상 우편 제목(rewardPostTitle)
* 보상 차트 이름(rewardName)
* 보상 차트의 inDate(rewardInDate)
* 리더보드 초기화 시 데이터 초기화 여부(isReset)
* 리더보드 정렬 방법(order)
* 리더보드에 사용한 테이블(table)
* 리더보드에 사용한 컬럼(column)

길드 리더보드는 추가 항목을 지원하지 않습니다. 공용 클래스인 `LeaderboardTableItem`에 선언된 `extraDataColumn`, `extraDataType`은 길드 리더보드에서 사용하지 않습니다.


### BackendLeaderboardTableReturnObject
```csharp

namespace BackEnd.Leaderboard
{
    public class LeaderboardTableItem
    {
        public string rankType;
        public string date;
        public bool isDivision;
        public string uuid;
        public string order;
        public string initializationTime;
        public bool isReset;
        public string title;
        public string table;
        public string column;

        // 일회성 리더보드에만 값이 담깁니다.
        // 누적 리더보드의 시작 시각과 2~6개월 주기의 날짜 정보는 JSON 응답으로 확인해 주세요.
        // 일간·주간·1개월 주기는 JSON 응답에도 두 필드가 포함되지 않습니다.
        public string rankStartDateAndTime;
        public string rankEndDateAndTime;

        // 유저 리더보드에서 추가 항목을 선택한 경우에만 값이 담깁니다.
        public string extraDataColumn;
        public string extraDataType;

        public Dictionary<string, string> rewardPostTitle = null;
    }


    public class BackendLeaderboardTableReturnObject : BackendReturnObject
    {
        public List<LeaderboardTableItem> GetLeaderboardTableList();
    }
}
```

## Example

### 동기
```csharp
BackEnd.Leaderboard.BackendLeaderboardTableReturnObject bro = null;
bro = Backend.Leaderboard.Guild.GetLeaderboards();

foreach (BackEnd.Leaderboard.LeaderboardTableItem item in bro.GetLeaderboardTableList())
{
    Debug.Log(item.ToString());
}
```

### 비동기
```csharp
Backend.Leaderboard.Guild.GetLeaderboards(bro =>
{
    foreach (BackEnd.Leaderboard.LeaderboardTableItem item in bro.GetLeaderboardTableList())
    {
        string uuid = item.uuid;
        Debug.Log(item.ToString());
    }
});
```

:::tip `LeaderboardTableItem` 사용 시 알림
`initializationTime` 등 시간을 나타내는 필드는 `string` 타입입니다.  
사용 목적에 맞게 시간대와 형식을 확인하여 파싱해 주세요.
:::
:::note JSON 응답과 `LeaderboardTableItem`의 차이
`initializationDay`, `initializationDate`, `inDate`, `rewardName`, `rewardInDate`, `isDatabase`는 JSON 응답에는 포함되지만 `LeaderboardTableItem`에는 없는 필드입니다.  
해당 값이 필요하면 `GetReturnValuetoJSON()`으로 직접 파싱해 주세요.

`rankStartDateAndTime`, `rankEndDateAndTime`은 클래스에 선언되어 있지만 일회성 리더보드에만 값이 담깁니다.  
누적 리더보드의 시작 시각과 2~6개월 주기의 날짜 정보는 `GetReturnValuetoJSON()`으로 확인해야 합니다. 필드의 값이 비어 있다고 해서 JSON 응답에도 값이 없는 것은 아닙니다.
:::

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조


### Error cases

**리더보드가 없는 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다

## GetReturnValuetoJSON
다음은 3개월 주기 길드 리더보드의 응답 예시입니다.

```js
{
    "rows": [
        {
            "rankType": "guild", // 리더보드 타입(user / guild)
            "isDivision": true, // 그룹 구분 여부
            "isDatabase": false, // 집계 대상이 데이터베이스 테이블인지 여부
            "order": "desc", // 정렬 순서

            // 리더보드 주기
            // day : 일간
            // week : 주간
            // month : 1개월
            // 2months, 3months, 4months, 5months, 6months : 2~6개월
            // infinity : 누적 리더보드
            // custom : 일회성 리더보드
            "date": "3months",

            "initializationTime": "02:00:00 UTC+09:00", // 초기화 시각
            "initializationDay": 1, // 주간 초기화 요일(0: 일요일 ~ 6: 토요일)
            "initializationDate": 15, // 1~6개월 주기의 초기화 일자(숫자 또는 "end")
            "rankStartDateAndTime": "2026-09-01T00:00:00.000Z", // 현재 회차의 시작 기준 월
            "rankEndDateAndTime": "2026-12-01T00:00:00.000Z", // 현재 회차의 종료 기준 월

            "inDate": "2024-08-20T07:08:09.478Z", // 리더보드 생성 일시
            "uuid": "01916e9d-5186-7594-b8ab-e286cefb6226",
            "isReset": true, // 초기화 시 데이터 리셋 여부

            "rewardName": "보상 차트 이름",
            "rewardInDate": "2024-07-30T04:52:13.448Z", // 보상 차트의 inDate
            "rewardPostTitle": { // 언어별 보상 우편 제목
                "ko": "한국보상",
                "en": "EnglishReward",
                "fallback": "en"
            },

            "title": "리더보드 제목",
            "table": "meta", // meta / goods
            "column": "리더보드에 사용된 컬럼 이름" // meta에 설정된 컬럼명 or totalGoods1Amount ~ totalGoods10Amount
        }
    ]
}
```

### 초기화 요일과 일자
* `initializationDay`는 주간 리더보드의 초기화 요일입니다. `0`은 일요일, `6`은 토요일이며, 주간 외 주기에서는 의미 없는 기본값 `1`이 반환됩니다.
* `initializationDate`는 1~6개월 주기 리더보드의 초기화 일자입니다. 특정 일자는 숫자, 말일은 문자열 `"end"`로 반환되므로 숫자로만 파싱하지 않도록 주의해 주세요. 일간·주간 주기에서는 의미 없는 기본값 `1`이 반환됩니다.

### 주기별 시작·종료 날짜
`rankStartDateAndTime`, `rankEndDateAndTime`의 JSON 응답은 주기에 따라 다음과 같이 달라집니다.

| 리더보드 주기 | JSON 응답의 날짜 정보 |
| :--- | :--- |
| 일회성(`custom`) | 실제 집계 시작·종료 시각 |
| 2~6개월(`2months`~`6months`) | 현재 회차의 시작·종료 기준 월. 각 월의 1일 00:00 UTC로 반환 |
| 일간(`day`)·주간(`week`)·1개월(`month`) | 두 필드 모두 반환되지 않음 |
| 누적(`infinity`) | 시작 시각(`rankStartDateAndTime`)만 제공 |

:::warning 2~6개월 주기의 날짜는 실제 집계 시각이 아닙니다
초기화 일자가 해당 월에 아직 도래하지 않았다면 직전 월이 기준 월로 반환됩니다.  
반환값에는 초기화 일자와 시각이 반영되지 않으므로, `rankStartDateAndTime`과 `rankEndDateAndTime`을 집계 기간 표시에 그대로 사용하면 안 됩니다.
:::
