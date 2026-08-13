---
sidebar_label: "랭킹과의 차이점"
position: "0.5"
description: "랭킹 기능과의 차이점"
---

# 랭킹 기능과의 차이점

## 그룹 구분된 리더보드 사용 여부

새롭게 추가된 Leaderboard의 경우, 그룹에 따른 리더보드 갱신을 할 수 있습니다.

```js
Param param = new Param();
param.Add("score", i);
param.Add("extraData", "추가 정보");

Backend.Leaderboard.User.UpdateMyDataAndRefreshLeaderboard("리더보드 uuid", "테이블 이름", "갱신할 row inDate", param);
```

그러나 URank 함수의 경우, 그룹에 속해 있어도 리더보드 갱신 함수 호출 시, NULL 그룹의 리더보드에 갱신됩니다.
* 그룹 구분하지 않은 리더보드는 정상적으로 등록이 가능합니다.

```js
Param param = new Param();
param.Add("score", i);
param.Add("extraData", "추가 정보");

Backend.URank.User.UpdateUserScore("랭킹 uuid", "테이블 이름", "갱신할 row inDate", param);
```

따라서 자신에 속한 그룹에서의 리더보드를 이용하고자 할 경우에는 Leaderboard 함수로 교체하셔야합니다.
* 그룹이 구분된 리더보드에 A 그룹에 속한 유저가 URank의 랭킹 갱신과 Leaderboard의 리더보드 갱신 함수를 사용할 경우, NULL그룹의 리더보드와 A 그룹의 리더보드 모두에게 랭킹이 등록될 수 있으니 주의해주시기 바랍니다.

## 리턴값 언마샬

새롭게 추가된 Leaderboard의 경우, 언마샬된 리턴값을 제공합니다.


### Backend.Leaderboard.User.GetLeaderboard
```js
{
    "rows": [
        {
            "gamerInDate": "2024-08-20T05:58:07.391Z",
            "nickname": "user99",
            "rank": 1,
            "index": 0,
            "score": "99"
            // 추가 항목이 존재할 경우
            "추가항목 컬럼명": "데이터"
        }
    ],
    "totalCount": 99
}
```

기존 URank의 경우, 데이터타입이 포함된 리턴값을 제공합니다.

### Backend.URank.User.GetRankList

```js
{
    "rows": [
       {
           // 유저의 게이머 inDate
           "gamerInDate": {
               "S": "2021-03-11T06:38:46.817Z"
           },
           // 유저의 닉네임
           "nickname": {
               "S": "닉네임s"
           },
           // 추가 항목
           // 컬럼명과 값이 그대로 노출됩니다.  
           // 추가 항목 컬럼명이 extraScore인 경우 extraScore
           "extraScore": {
               "N": "4577"
           },
           // 점수
           // 컬럼명이 score로 통일됩니다.  
           // 랭킹 항목 컬럼명이 power인 경우에도 score
           "score": {
               "N": "199"
           },
           // offset
           "index": {
               "N": 0
           },
           // 유저의 랭킹
           "rank": {
               "N": 1
           }
       },
       // and etc...  
    ],
    // 랭킹에 등록된 총 랭커의 수
    "totalCount":100
}
```


## 에러 케이스 변경

새롭게 추가된 Leaderboard의 경우, 에러 케이스가 leaderboard에 관한 내용으로 변경되었습니다.

**uuid가 null 혹은 string.empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : leaderboardUuid is null or string.Empty

**존재하지 않는 uuid로 조회를 시도한 경우**  
StatusCode : 404  
ErrorCode : NotFoundException  
Message : leaderboard not found, leaderboard을(를) 찾을 수 없습니다

기존 URank의 경우에는 다음과 같았습니다.

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**존재하지 않는 uuid 일 때**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다
