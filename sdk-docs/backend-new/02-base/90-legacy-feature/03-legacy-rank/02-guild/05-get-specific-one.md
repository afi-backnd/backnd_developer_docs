---
sidebar_label: 특정 길드의 랭킹 조회
---

# GetGuildRank
public Task&lt;RequestResult&gt; **GetGuildRankAsync**(string **rankUuid**, string **guildIndate**);  
public Task&lt;RequestResult&gt; **GetGuildRankAsync**(string **rankUuid**, string **guildIndate**, int **gap**);

:::info 기능 개선 안내
기존 랭킹의 성능과 기능을 개선한 **리더보드** 기능을 제공하고 있습니다.  
**콘솔에서 신규 생성은 리더보드만 제공**하므로 해당 기능을 사용해 주세요.
:::

:::warning 그룹 구분된 리더보드 사용 불가
URank 랭킹 함수는 그룹이 구분된 리더보드에서 NULL그룹만 조회할 수 있습니다.  
그룹에 따른 리더보드를 확인하고자 할 경우에는 Leaderboard함수를 이용해주세요. 
:::

## 파라미터
| Value        | Type           | Description  | Default | 
| :------------ |:-------------| :----- | :--- |
| rankUuid | string | 조회할 랭킹의 uuid | - | 
| guildIndate | string | 조회할 길드의 inDate | - |
| gap | int | 함께 조회할 위아래 길드의 수(0 ~ 25) | 0 |

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 길드 랭킹 정보 조회](/sdk-docs/backend/base/rank/guild/get-all-settings) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값과 guildIndate를 이용하여 해당 랭킹에서 해당 길드의 랭킹을 조회합니다.  
* 동점(랭킹이 같은) 길드가 존재할 경우 gap 함수를 통해 조회 시 동일한 랭킹의 길드가 리턴될 수 있습니다. 

## Example

### Task 방식
```js
// 자신이 가입한 길드의 랭킹만 조회
var reqResult = await BackndLegacy.URank.Guild.GetGuildRankAsync("rankUuid", "guildIndate");

// 위아래 3개의 길드를 포함하여 랭킹 조회
// 자신의 길드가 4등인 경우 1 ~ 7등 길드가 조회됨
var reqResult2 = await BackndLegacy.URank.Guild.GetGuildRankAsync("rankUuid", "guildIndate", 3);
```

### Callback 방식
```js
// 자신이 가입한 길드의 랭킹만 조회
BackndLegacy.URank.Guild.GetGuildRank("rankUuid", "guildIndate", callback =>
{
    // 이후 처리
});

// 위아래 3개의 길드를 포함하여 랭킹 조회
// 자신의 길드가 4등인 경우 1 ~ 9등 길드가 조회됨
BackndLegacy.URank.Guild.GetGuildRank("rankUuid", "guildIndate", 5, callback =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**해당 길드의 랭킹 조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

### Error cases

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**guildIndate가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : guildIndate is null or empty

**존재하지 않는 uuid로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

**존재하지 않는 guildIndate로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다

**해당 길드가 랭킹에 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guildRank not found, guildRank을(를) 찾을 수 없습니다

## ReturnValueJson
### gap이 0인 경우
```js
{
    "rows": [
        {
            // 길드 이름
            "guildName": {
                "S": "guildName"
            },
            // 길드의 inDate
            "guildInDate": {
                "S": "2021-03-11T03:23:24.913Z"
            },
            // 길드의 점수
            // 메타 랭킹, 굿즈 랭킹 관계없이 모두 score입니다.  
            "score": {
                "N": 9999
            },
            // offset 
            "index": {
                "N": 0
            },
            // 길드의 랭킹
            "rank": {
                "N": 1
            }
        }
    ],
    // 랭킹에 등록된 총 길드의 수
    "totalCount": 100
}
```

### gap이 1 인 경우
```js
{
    "rows": [
        // 검색한 길드보다 랭킹이 1 낮은 유저
        {
            "guildName": {
                "S": "guildName0"
            },
            "guildInDate": {
                "S": "2021-03-11T03:23:24.913Z"
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
        },
        // 검색한 길드
        {
            "guildName": {
                "S": "guildName1"
            },
            "guildInDate": {
                "S": "2021-03-11T03:23:24.912Z"
            },
            "score": {
                "N": 9999
            },
            "index": {
                "N": 1
            },
            "rank": {
                "N": 2
            }
        },
        // 검색한 길드보다 랭킹이 1 낮은 유저
         {
            "guildName": {
                "S": "guildName2"
            },
            "guildInDate": {
                "S": "2021-03-11T03:23:24.900Z"
            },
            "score": {
                "N": 9998
            },
            "index": {
                "N": 2
            },
            "rank": {
                "N": 3
            }
        }
    ],
    "totalCount": 100
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
public async Task GetGuildRankTest()
{
    string guildUUID = "";
    string guildIndate = "";

    List<GuildRankItem> rankItemList = new List<GuildRankItem>();        
    var reqResult = await BackndLegacy.URank.Guild.GetGuildRankAsync(guildUUID, guildIndate, 3);
    if (reqResult.IsSuccess())
    {
        string extraName = string.Empty;            
        var rankListJson = reqResult.GetJObject();
        var rankRows = reqResult.GetRows();
        for (int i = 0; i < rankRows.Count; i++)
        {
            GuildRankItem rankItem = new GuildRankItem();

            rankItem.guildInDate = rankRows[i]["guildInDate"].ToString();
            rankItem.guildName = rankRows[i]["guildName"].ToString();
            rankItem.score = rankRows[i]["score"].ToString();
            rankItem.index = rankRows[i]["index"].ToString();
            rankItem.rank = rankRows[i]["rank"].ToString();
            rankItem.totalCount = rankListJson["totalCount"].ToString();

            rankItemList.Add(rankItem);

            Debug.Log(rankItem.ToString());
        }
    }
}
```
