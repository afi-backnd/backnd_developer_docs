---
sidebar_label: "길드 랭킹 리스트 조회"
draft: "true"
unlisted: "true"
description: "GetRankList"
---

# GetRankList
public BackendReturnObject **GetRankList**(string **rankUuid**);  
public BackendReturnObject **GetRankList**(string **rankUuid**, int **limit**);  
public BackendReturnObject **GetRankList**(string **rankUuid**, int **limit**, int **offset**);

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
| limit | int | 조회할 길드의 수(1 ~ 50) | 10 |
| offset | int | 조회할 랭킹의 시작점(0 이상) | 0 |

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 길드 랭킹 정보 조회](/sdk-docs/backend/base/rank/guild/get-all-settings) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹에 등록된 길드들의 리스트를 조회합니다.  


## Example

### 동기
```js
// rankUuid 랭킹에서 1 ~ 10등 길드 조회
Backend.URank.Guild.GetRankList("rankUuid");
// rankUuid 랭킹에서 1 ~ 100등 길드 조회
Backend.URank.Guild.GetRankList("rankUuid", 100);
// rankUuid 랭킹에서 11등 ~ 20등 길드 조회
Backend.URank.Guild.GetRankList("rankUuid", 10, 10);
```

### 비동기
```js
// rankUuid 랭킹에서 1 ~ 10등 길드 조회
Backend.URank.Guild.GetRankList("rankUuid", callback=> {
    // 이후 처리
});
// rankUuid 랭킹에서 1 ~ 100등 길드 조회
Backend.URank.Guild.GetRankList("rankUuid", 100, callback=> {
    // 이후 처리
});
// rankUuid 랭킹에서 11등 ~ 20등 길드 조회
Backend.URank.Guild.GetRankList("rankUuid", 10, 10, callback=> {
    // 이후 처리
});
```

### SendQueue
```js
// rankUuid 랭킹에서 1 ~ 10등 길드 조회
SendQueue.Enqueue(Backend.URank.Guild.GetRankList, "rankUuid", callback=> {
    // 이후 처리
});
// rankUuid 랭킹에서 1 ~ 100등 길드 조회
SendQueue.Enqueue(Backend.URank.Guild.GetRankList, "rankUuid", 100, callback=> {
    // 이후 처리
});
// rankUuid 랭킹에서 11등 ~ 20등 길드 조회
SendQueue.Enqueue(Backend.URank.Guild.GetRankList, "rankUuid", 10, 10, callback=> {
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

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**존재하지 않는 uuid 일 때**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

## GetReturnValuetoJSON
## GetReturnValuetoJSON
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
                "N": "9999"
            },
            // offset 
            "index": {
                "N": 0
            },
            // 길드의 랭킹
            "rank": {
                "N": 1
            }
        },
        // and etc...  
    ],
    // 랭킹에 등록된 총 길드의 수
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
public void GetRankListTest()
{
    int limit = 43;

    List<GuildRankItem> rankItemList = new List<GuildRankItem>();

    BackendReturnObject bro = Backend.URank.Guild.GetRankList(userUuid, limit);

    if(bro.IsSuccess())
    {
        LitJson.JsonData rankListJson = bro.GetFlattenJSON();
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
    }
}
```
