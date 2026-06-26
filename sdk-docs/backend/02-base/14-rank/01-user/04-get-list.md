---
sidebar_label: 유저 랭킹 리스트 조회
draft: true
unlisted: true
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
| limit | int | 조회할 랭커의 수(1 ~ 50) | 10 |
| offset | int | 조회할 랭킹의 시작점(0 이상) | 0 |

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 유저 랭킹 정보 조회](/sdk-docs/backend/base/rank/user/get-all-sertting) 함수를 이용하여 uuid 값 확인

## 설명
uuid 값을 이용하여 해당 랭킹에 등록된 랭커들의 리스트를 조회합니다.  
> 공동 순위의 경우, 랭킹 정렬 기준(오름차순, 내림차순)과 같은 기준으로 gamer_id(회원번호)를 사전식 순서로 정렬합니다.  

## Example

### 동기
```js
// rankUuid 랭킹에서 1 ~ 10등 랭커 조회
Backend.URank.User.GetRankList("rankUuid");
// rankUuid 랭킹에서 1 ~ 50등 랭커 조회
Backend.URank.User.GetRankList("rankUuid", 50);
// rankUuid 랭킹에서 11등 ~ 20등 랭커 조회
Backend.URank.User.GetRankList("rankUuid", 10, 10);
```

### 비동기
```js
// rankUuid 랭킹에서 1 ~ 10등 랭커 조회
Backend.URank.User.GetRankList("rankUuid", callback=> {
    // 이후 처리
});
// rankUuid 랭킹에서 1 ~ 50등 랭커 조회
Backend.URank.User.GetRankList("rankUuid", 50, callback=> {
    // 이후 처리
});
// rankUuid 랭킹에서 11등 ~ 20등 랭커 조회
Backend.URank.User.GetRankList("rankUuid", 10, 10, callback=> {
    // 이후 처리
});
```

### SendQueue
```js
// rankUuid 랭킹에서 1 ~ 10등 랭커 조회
SendQueue.Enqueue(Backend.URank.User.GetRankList, "rankUuid", callback=> {
    // 이후 처리
});
// rankUuid 랭킹에서 1 ~ 50등 랭커 조회
SendQueue.Enqueue(Backend.URank.User.GetRankList, "rankUuid", 50, callback=> {
    // 이후 처리
});
// rankUuid 랭킹에서 11등 ~ 20등 랭커 조회
SendQueue.Enqueue(Backend.URank.User.GetRankList, "rankUuid", 10, 10, callback=> {
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
public void GetMyRankTest()
{
    string userUuid = "81272320-8c40-112b-b174-d9233f6bd0e8";

    int limit = 100;

    List<RankItem> rankItemList = new List<RankItem>();

    BackendReturnObject bro = Backend.URank.User.GetRankList(userUuid, limit);

    if(bro.IsSuccess())
    {
        LitJson.JsonData rankListJson = bro.GetFlattenJSON();

        string extraName = string.Empty;

        for(int i = 0; i < rankListJson["rows"].Count; i++)
        {
            RankItem rankItem = new RankItem();

            rankItem.gamerInDate = rankListJson["rows"][i]["gamerInDate"].ToString();
            rankItem.nickname = rankListJson["rows"][i]["nickname"].ToString();
            rankItem.score = rankListJson["rows"][i]["score"].ToString();
            rankItem.index = rankListJson["rows"][i]["index"].ToString();
            rankItem.rank = rankListJson["rows"][i]["rank"].ToString();
            rankItem.totalCount = rankListJson["totalCount"].ToString();

            if(rankListJson["rows"][i].ContainsKey(rankItem.extraName))
            {
                rankItem.extraData = rankListJson["rows"][i][rankItem.extraName].ToString();
            }

            rankItemList.Add(rankItem);
            Debug.Log(rankItem.ToString());
        }
    }
}
```
