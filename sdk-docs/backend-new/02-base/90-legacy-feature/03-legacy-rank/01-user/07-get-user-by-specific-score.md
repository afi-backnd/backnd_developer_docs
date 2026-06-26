---
sidebar_label: 특정 점수를 가진 유저 랭킹 조회
description: "특정 점수를 가진 유저 랭킹 조회"
---

# GetRankListByScore
public Task&lt;RequestResult&gt; **GetRankListByScoreAsync**(string **rankUuid**, int **score**);  
public Task&lt;RequestResult&gt; **GetRankListByScoreAsync**(string **rankUuid**, long **score**);

:::info 기능 개선 안내
기존 랭킹의 성능과 기능을 개선한 **리더보드** 기능을 제공하고 있습니다.  
**콘솔에서 신규 생성은 리더보드만 제공**하므로 해당 기능을 사용해 주세요.
:::

:::warning 그룹 구분된 리더보드 사용 불가
URank 랭킹 함수는 그룹이 구분된 리더보드에서 NULL그룹만 조회할 수 있습니다.  
그룹에 따른 리더보드를 확인하고자 할 경우에는 Leaderboard함수를 이용해주세요. 
:::

## 파라미터
| Value        | Type           | Description  |  
| :------------ |:-------------| :----- | 
| rankUuid | string | 조회할 랭킹의 uuid |  
| score | int/long | 조회할 점수 | 

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 유저 랭킹 정보 조회](/sdk-docs/backend/base/rank/user/get-all-sertting) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹에 등록된 랭커들 중 score 값을 가지고 있는 랭커와 해당 점수 위, 아래 점수의 랭커를 조회합니다.  
* 조회한 score의 유저가 여러 명 존재할 경우 모든 유저가 조회됩니다.  
* 해당 score의 위, 아래 점수의 유저가 여러 명 존재하더라도 각각 단 한 명의 유저만 조회됩니다.  

## Example

### Task 방식
```js
var score = 0; //조회할 점수
var reqResult = await BackndLegacy.URank.User.GetRankListByScoreAsync("rankUuid", score);
```

### Callback 방식
```js
var score = 0; //조회할 점수
BackndLegacy.URank.User.GetRankListByScore("rankUuid", score, callback=>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

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
message : rankUuid is null or empty

**존재하지 않는 uuid로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

## ReturnValueJson
### 해당 점수의 유저와 위, 아래 점수를 가진 유저가 모두 존재하는 경우
```js
{
    "rows": [
        {
            // 유저 inDate
            "gamerInDate": {
                "S": "2021-03-10T06:01:05.098Z"
            },
            // 유저 닉네임
            "nickname": {
                "S": "닉네임0호"
            },
            // 추가 항목
           // 컬럼명과 값이 그대로 노출됩니다.  
           // 추가 항목 컬럼명이 CONNECTION_MIN인 경우 CONNECTION_MIN
            "CONNECTION_MIN": {
                "S": "6P0LIECBQIEV"
            },
            // 점수
            // 컬럼명이 score로 통일됩니다.  
            // 랭킹 항목 컬럼명이 power인 경우에도 score
            "score": {
                "N": 100
            },
            // offset
            "index": {
                "N": 10
            },
            // 유저의 랭킹
            "rank": {
                "N": 9
            }
        },
        // and etc...  
    ],
    // previousRank가 존재하지 않는 경우
    "previousRank": {
        "NULL": true
    },
    // previousRank가 존재하는 경우
    // 단 1명의 유저만 조회됩니다.  
    "previousRank": [
        {
            "gamerInDate": {
                "S": "2021-03-10T06:02:36.416Z"
            },
            "nickname": {
                "S": "닉네임10호"
            },
            "CONNECTION_MIN": {
                "S": "SS3P9KH58U92"
            },
            "score": {
                "N": 101
            },
            "index": {
                "N": 9
            },
            "rank": {
                "N": 8
            }
        }
    ],
    // nextRank가 존재하지 않는 경우
    "nextRank": {
        "NULL": true
    },
    // nextRank가 존재하는 경우
    // 단 1명의 유저만 조회됩니다.  
    "nextRank": [
        {
            "gamerInDate": {
                "S": "2021-03-10T06:02:36.415Z"
            },
            "nickname": {
                "S": "닉네임100호"
            },
            "CONNECTION_MIN": {
                "S": "SS3P9KH58T33"
            },
            "score": {
                "N": "99"
            },
            "index": {
                "N": 11
            },
            "rank": {
                "N": 10
            }
        }
    ],
    // 랭킹에 존재하는 모든 랭커의 수
    "totalCount": 907
}
```

### 해당 점수를 가진 유저는 없지만, 해당 점수보다 높은 점수를 가진 유저는 존재하는 경우
```js
{
    "rows": [ ],
    // previousRank가 존재하는 경우
    // 단 1명의 유저만 조회됩니다.  
    "previousRank": [
        {
            "gamerInDate": {
                "S": "2021-03-10T06:02:36.416Z"
            },
            "nickname": {
                "S": "닉네임10호"
            },
            "CONNECTION_MIN": {
                "S": "SS3P9KH58U92"
            },
            "score": {
                "N": 101
            },
            "index": {
                "N": 9
            },
            "rank": {
                "N": 8
            }
        }
    ],
    // nextRank가 존재하지 않는 경우
    "nextRank": {
        "NULL": true
    },
    // 랭킹에 존재하는 모든 랭커의 수
    "totalCount": 907
}
```

## Sample Code
```js
public class RankItem
{
    public string gamerInDate;
    public string nickname;
    public string score;
    public string index;
    public string rank;
    public string extraData = string.Empty;
    public string extraName = string.Empty;
    public string totalCount;

    public override string ToString()
    {
        string str = $"유저인데이트:{gamerInDate}\n닉네임:{nickname}\n점수:{score}\n정렬:{index}\n순위:{rank}\n총합:{totalCount}\n";
        if(extraName != string.Empty)
        {
            str += $"{extraName}:{extraData}\n";
        }
        return str;
    }
}
```

```js
public async Task GetRankListByScoreTest()
{
    string userUuid = "81272320-8c40-112b-b174-d9233f6bd0e8";
    int score = 1835242435;

    List<RankItem> rankItemList = new List<RankItem>();

    var reqResult = await BackndLegacy.URank.User.GetRankListByScoreAsync(userUuid, score);
    if (reqResult.IsSuccess())
    {
        var rankListJson = reqResult.GetJObject();
        var prevRows = reqResult.GetRows("previousRank");
        for (int i = 0; i < prevRows.Count; i++)
        {
            RankItem rankItem = new RankItem();

            rankItem.gamerInDate = prevRows[i]["gamerInDate"].ToString();
            rankItem.nickname = prevRows[i]["nickname"].ToString();
            rankItem.score = prevRows[i]["score"].ToString();
            rankItem.index = prevRows[i]["index"].ToString();
            rankItem.rank = prevRows[i]["rank"].ToString();
            rankItem.totalCount = rankListJson["totalCount"].ToString();

            var jobj = prevRows[i] as JObject;
            if (jobj.ContainsKey(rankItem.extraName))
            {
                rankItem.extraData = prevRows[i][rankItem.extraName].ToString();
            }

            rankItemList.Add(rankItem);
            Debug.Log(rankItem.ToString());
        }

        var rows = reqResult.GetRows("rows");
        for (int i = 0; i < rows.Count; i++)
        {
            RankItem rankItem = new RankItem();

            rankItem.gamerInDate = rows[i]["gamerInDate"].ToString();
            rankItem.nickname = rows[i]["nickname"].ToString();
            rankItem.score = rows[i]["score"].ToString();
            rankItem.index = rows[i]["index"].ToString();
            rankItem.rank = rows[i]["rank"].ToString();
            rankItem.totalCount = rankListJson["totalCount"].ToString();

            var jobj = rows[i] as JObject;
            if (jobj.ContainsKey(rankItem.extraName))
            {
                rankItem.extraData = rows[i][rankItem.extraName].ToString();
            }

            rankItemList.Add(rankItem);
            Debug.Log(rankItem.ToString());
        }

        var nextRows = reqResult.GetRows("nextRank");
        for (int i = 0; i < nextRows.Count; i++)
        {
            RankItem rankItem = new RankItem();

            rankItem.gamerInDate = nextRows[i]["gamerInDate"].ToString();
            rankItem.nickname = nextRows[i]["nickname"].ToString();
            rankItem.score = nextRows[i]["score"].ToString();
            rankItem.index = nextRows[i]["index"].ToString();
            rankItem.rank = nextRows[i]["rank"].ToString();
            rankItem.totalCount = rankListJson["totalCount"].ToString();

            var jobj = nextRows[i] as JObject;
            if (jobj.ContainsKey(rankItem.extraName))
            {
                rankItem.extraData = nextRows[i][rankItem.extraName].ToString();
            }

            rankItemList.Add(rankItem);
            Debug.Log(rankItem.ToString());
        }
    }
}
```
