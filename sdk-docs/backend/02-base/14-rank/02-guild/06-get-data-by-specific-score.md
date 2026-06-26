---
sidebar_label: 특정 점수를 통한 길드 랭킹 조회
draft: true
unlisted: true
---

# GetRankListByScore
BackendReturnObject **GetRankListByScore**(string **rankUuid**, int **score**);  
BackendReturnObject **GetRankListByScore**(string **rankUuid**, long **score**);

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
* [모든 길드 랭킹 정보 조회](/sdk-docs/backend/base/rank/guild/get-all-settings) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹에 등록된 길드들 중 score 값을 가지고 있는 길드와 해당 점수 위, 아래 점수의 길드를 조회합니다.  
* 조회한 score의 길드가 여러 개 존재할 경우 모든 길드가 조회됩니다.  
* 해당 score의 위, 아래 점수의 길드가 여러 개 존재하더라도 각각 단 한 개의 길드만 조회됩니다.  

## Example

### 동기
```js
var score = 0; //조회할 점수
Backend.URank.Guild.GetRankListByScore("rankUuid", score);
```

### 비동기
```js
var score = 0; //조회할 점수
Backend.URank.Guild.GetRankListByScore("rankUuid", score, callback=> {
    // 이후 처리
});
```

### SendQueue
```js
var score = 0; //조회할 점수
SendQueue.Enqueue(Backend.URank.Guild.GetRankListByScore, "rankUuid", score, callback => {
    // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**조회에 성공하였으나 랭킹의 길드가 등록되지 않은 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[],"previousRank":{"NULL":true},"nextRank":{"NULL":true},"totalCount":0}

**조회에 성공하였으나 해당 점수의 길드가 존재하지 않는 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**존재하지 않는 uuid로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

### 해당 점수의 길드와 위, 아래 점수를 가진 길드가 모두 존재하는 경우
```js
{
    "rows": [
        {
            // 길드명
            "guildName": {
                "S": "guilds0"
            },
            // 길드 inDate
            "guildInDate": {
                "S": "2021-03-11T03:23:24.914Z"
            },
            // 점수
            // 메타 랭킹, 굿즈 랭킹 관계없이 모두 score입니다.  
            "score": {
                "N": "9999"
            },
            // offset
            "index": {
                "N": 1
            },
            // 길드의 랭킹
            "rank": {
                "N": 2
            },
            // and etc... 
            // 동점자가 존재할 경우 함께 리턴됩니다.  
        }
    ],
    // previousRank가 존재하지 않는 경우
    "previousRank": {
        "NULL": true
    },
    // previousRank가 존재하는 경우
    // 단 1개의 길드만 조회됩니다.  
    "previousRank": [
        {
            "guildName": {
                "S": "guilds1"
            },
            "guildInDate": {
                "S": "2021-03-11T03:24:06.402Z"
            },
            "score": {
                "N": 10000
            },
            "index": {
                "N": 0
            },
            "rank": {
                "N": 1
            }
        }
    ],
    // nextRank가 존재하지 않는 경우
    "nextRank": {
        "NULL": true
    },
    // nextRank가 존재하는 경우
    // 단 1개의 길드만 조회됩니다.  
    "nextRank": [
        {
            "guildName": {
                "S": "guilds2"
            },
            "guildInDate": {
                "S": "2021-03-11T03:24:05.378Z"
            },
            "score": {
                "N": 5000
            },
            "index": {
                "N": 2
            },
            "rank": {
                "N": 3
            }
        }
    ],
    // 랭킹에 등록된 전체 길드의 수
    "totalCount": 3
}
```

### 해당 점수를 가진 길드는 없지만, 해당 점수보다 높은 점수를 가진 길드는 존재하는 경우
```js
{
    "rows": [ ],
   // previousRank가 존재하는 경우
    // 단 1개의 길드만 조회됩니다.  
    "previousRank": [
        {
            "guildName": {
                "S": "guilds1"
            },
            "guildInDate": {
                "S": "2021-03-11T03:24:06.402Z"
            },
            "score": {
                "N": 10000
            },
            "index": {
                "N": 0
            },
            "rank": {
                "N": 1
            }
        }
    ],
    // nextRank가 존재하지 않는 경우
    "nextRank": {
        "NULL": true
    },
    // 랭킹에 존재하는 모든 길드의 수
    "totalCount": 907
}
```

## Sample Code
```js
public class GuildRankItem
{
    public string guildInDate;
    public string guildName;
    public string score;
    public string index;
    public string rank;
    public string totalCount;

    public override string ToString()
    {
        return $"길드인데이트:{guildInDate}\n길드이름:{guildName}\n점수:{score}\n정렬:{index}\n순위:{rank}\n총합:{totalCount}\n";
    }
}
```

```js
public void GetRankListByScoreTest()
{
    SDKTester.InitializeFullTest();
    int score = 1815817124;

    List<GuildRankItem> rankItemList = new List<GuildRankItem>();

    BackendReturnObject bro = Backend.URank.Guild.GetRankListByScore(guildRankUUID, score);

    if(bro.IsSuccess())
    {
        LitJson.JsonData rankListJson = bro.GetFlattenJSON();

        for(int i = 0; i < rankListJson["previousRank"].Count; i++)
        {
            GuildRankItem rankItem = new GuildRankItem();

            rankItem.guildInDate = rankListJson["previousRank"][i]["guildInDate"].ToString();
            rankItem.guildName = rankListJson["previousRank"][i]["guildName"].ToString();
            rankItem.score = rankListJson["previousRank"][i]["score"].ToString();
            rankItem.index = rankListJson["previousRank"][i]["index"].ToString();
            rankItem.rank = rankListJson["previousRank"][i]["rank"].ToString();
            rankItem.totalCount = rankListJson["totalCount"].ToString();

            rankItemList.Add(rankItem);
            Debug.Log(rankItem.ToString());
        }

        for(int i = 0; i < rankListJson["rows"].Count; i++)
        {
            GuildRankItem rankItem = new GuildRankItem();

            rankItem.guildInDate = rankListJson["rows"][i]["guildInDate"].ToString();
            rankItem.guildName = rankListJson["rows"][i]["guildName"].ToString();
            rankItem.score = rankListJson["rows"][i]["score"].ToString();
            rankItem.index = rankListJson["rows"][i]["index"].ToString();
            rankItem.rank = rankListJson["rows"][i]["rank"].ToString();
            rankItem.totalCount = rankListJson["totalCount"].ToString();

            rankItemList.Add(rankItem);
            Debug.Log(rankItem.ToString());
        }
        for(int i = 0; i < rankListJson["nextRank"].Count; i++)
        {
            GuildRankItem rankItem = new GuildRankItem();

            rankItem.guildInDate = rankListJson["nextRank"][i]["guildInDate"].ToString();
            rankItem.guildName = rankListJson["nextRank"][i]["guildName"].ToString();
            rankItem.score = rankListJson["nextRank"][i]["score"].ToString();
            rankItem.index = rankListJson["nextRank"][i]["index"].ToString();
            rankItem.rank = rankListJson["nextRank"][i]["rank"].ToString();
            rankItem.totalCount = rankListJson["totalCount"].ToString();

            rankItemList.Add(rankItem);
            Debug.Log(rankItem.ToString());
        }
    }
}
```
