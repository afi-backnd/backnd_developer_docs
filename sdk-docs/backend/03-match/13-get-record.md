---
sidebar_label: "매칭 기록 조회"
description: "GetMatchRecord"
---

# GetMatchRecord

public BackendReturnObject **GetMatchRecord**(string **gamerIndate**, MatchType **matchType**, MatchModeType **matchModeType**, string **matchCardIndate**);  
public BackendReturnObject **GetMatchRecord**(string **gamerIndate**, MatchType **matchType**, MatchModeType **matchModeType**, string **matchCardIndate**, int **limit**);  
public BackendReturnObject **GetMatchRecord**(string **gamerIndate**, MatchType **matchType**, MatchModeType **matchModeType**, string **matchCardIndate**, int **limit**, string **offset**);

## 파라미터

| Value           | Type          | Description                 |
| :-------------- | :------------ | :-------------------------- |
| gamerIndate     | string        | 조회할 유저 inDate          |
| matchType       | MatchType     | 조회할 매치 타입            |
| MatchModeType   | MatchModeType | 조회할 매치 모드 타입       |
| matchCardIndate | string        | 조회할 매칭 카드 inDate     |
| limit           | int           | (optional) 조회할 시즌 개수 |
| offset          | string        | (optional) 페이징 시작 점   |

## 설명

해당 유저의 특정 매치에서의 매칭 기록을 조회합니다.  

- 콘솔에서 '기간 없음' 매칭을 생성한 경우 모든 매칭 기록이 하나의 row에 모두 포함되어 로드됩니다.  
- 콘솔에서 기간을 설정했을 경우 각 기간(시즌) 별로 row가 배정됩니다.  
- rows는 가장 최근 기간순으로 로드됩니다.  
  > 기간이 일주일로 설정되고, 한 달 가량의 매칭 기록이 있을 때 0번 인덱스에 4주 차, 1번 인덱스에 3주 차... 순으로 배열이 생성됩니다.  

## Example

### 동기

```js
Backend.Match.GetMatchRecord(gamerIndate, MatchType.Random, MatchModeType.OneOnOne, matchCardIndate);

Backend.Match.GetMatchRecord(gamerIndate, MatchType.Random, MatchModeType.OneOnOne, matchCardIndate, 10);

Backend.Match.GetMatchRecord(gamerIndate, MatchType.Random, MatchModeType.OneOnOne, matchCardIndate, 10, offset);
```

### 비동기

```js
Backend.Match.GetMatchRecord(gamerIndate, MatchType.Random, MatchModeType.OneOnOne, matchCardIndate, (callback) => {
    //todo
});

Backend.Match.GetMatchRecord(gamerIndate, MatchType.Random, MatchModeType.OneOnOne, matchCardIndate, 10, (callback) => {
    //todo
});

Backend.Match.GetMatchRecord(gamerIndate, MatchType.Random, MatchModeType.OneOnOne, matchCardIndate, 10, offset, (callback) => {
    //todo
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Match.GetMatchRecord, gamerIndate, MatchType.Random,MatchModeType.OneOnOne, matchCardIndate, (callback) => {
    //todo
});

SendQueue.Enqueue(Backend.Match.GetMatchRecord, gamerIndate, MatchType.Random, MatchModeType.OneOnOne, matchCardIndate, 10, (callback) => {
    //todo
});

SendQueue.Enqueue(Backend.Match.GetMatchRecord, gamerIndate, MatchType.Random, MatchModeType.OneOnOne, matchCardIndate, 10, offset, (callback) => {
    //todo
});
```

## ReturnCase

### Success cases

**기록이 존재하는 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**기록이 없는 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

## GetReturnValuetoJSON

```js
{
    // 랜덤
    rows:
    [
        {
            victory  : { N : "1" },
            draw : { N : "1" },
            defeat : { N : "1" },
            partition : { S : "_match_records_random_OneOnOne_" }, // 호출한 매칭 타입
            inDate : { S : "2019-12-08" }, // 시즌 시작 날짜('기간 없음'의 경우 매칭 카드 생성 날짜)
            updatedAt : { S : "2019-12-10T05:32:44.985Z" }, // 마지막 매치메이킹 진행 날짜
            nickname : { S : "theBackend" },
            championship:{ N : "1" } // 개인전(Melee) 검색 시에만 발생. 1등을 한 횟수
        },
        {
            victory: [Object],
            draw: [Object],
            defeat: [Object],
            partition: [Object],
            inDate: [Object],
            updatedAt: [Object],
            nickname: [Object],
            championship:[Object]
        },
    ],

    // 포인트
    rows:
    [
        {
            point : { N : "1000" }
            victory  : { N : "1" },
            draw : { N : "1" },
            defeat : { N : "1" },
            partition : { S : "_match_records_point_OneOnOne_" }, // 호출한 매칭 타입
            inDate : { S : "2019-12-08" } // 시즌 시작 날짜('기간 없음'의 경우 매칭 카드 생성 날짜)
            updatedAt : { S : "2019-12-10T05:32:44.985Z" } // 마지막 매치메이킹 진행 날짜
            nickname : { S : "theBackend" }
            championship:{ N : "1" } // 개인전(Melee) 검색 시에만 발생. 1등을 한 횟수
        },
        {
            point: [Object],
            victory: [Object],
            draw: [Object],
            defeat: [Object],
            partition: [Object],
            inDate: [Object],
            updatedAt: [Object],
            nickname: [Object],
            championship:[Object]
        },
    ],

    // MMR
    rows:
    [
        {
            mmr : { N : "1000" }
            victory  : { N : "1" },
            draw : { N : "1" },
            defeat : { N : "1" },
            partition : { S : "_match_records_mmr_OneOnOne_" }, // 호출한 매칭 타입
            inDate : { S : "2019-12-08" } // 시즌 시작 날짜('기간 없음'의 경우 매칭 카드 생성 날짜)
            updatedAt : { S : "2019-12-10T05:32:44.985Z" } // 마지막 매치메이킹 진행 날짜
            nickname : { S : "theBackend" }
        },
        {
            mmr: [Object],
            victory: [Object],
            draw: [Object],
            defeat: [Object],
            partition: [Object],
            inDate: [Object],
            updatedAt: [Object],
            nickname: [Object],
        },
    ],
}
```
