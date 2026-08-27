---
sidebar_label: "특정 유저 랭킹 조회"
draft: "true"
unlisted: "true"
description: "GetUserRank"
---

# GetUserRank
public BackendReturnObject **GetUserRank**(string **rankUuid**, string **userInDate**);  
public BackendReturnObject **GetUserRank**(string **rankUuid**, string **userInDate**, int **gap**);

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
| userIndate | string | 조회할 유저의 inDate | - |
| gap | int | 함께 조회할 위아래 랭커의 수(0 ~ 25) | 0 |

rankUuid 값은 아래 방법을 통해 확인할 수 있습니다.  
* uuid 값은 뒤끝 콘솔에서 랭킹을 생성 후 해당 랭킹 정보에서 uuid 값 확인
* [모든 유저 랭킹 정보 조회](/sdk-docs/backend/base/leaderboard/user/get-leaderboard) 함수를 이용하여 uuid 값 확인

userIndate 값은 아래 방법을 통해 확인할 수 있습니다.  
* 친구의 inDate 확인
* 길드원의 inDate 확인
* public 테이블의 owner_inDate 컬럼을 통해 확인
* 기타 유저의 inDate를 공유하는 방법 등을 통해 확인

## 설명
uuid 값과 userIndate를 이용하여 해당 랭킹에서 해당 유저의 랭킹을 조회합니다.  
* 동점(랭킹이 같은) 유저가 존재할 경우 gap 함수를 통해 조회 시 동일한 랭킹의 유저가 리턴될 수 있습니다. 
## Example

### 동기
```js
// 해당 유저의 랭킹만 조회
Backend.URank.User.GetUserRank("rankUuid", "userIndate");

// 위아래 3명의 유저를 포함하여 랭킹 조회
// 해당 유저가 4등인 경우 1 ~ 7등 유저가 조회됨
Backend.URank.User.GetUserRank("rankUuid", "userIndate", 3);
```

### 비동기
```js
// 해당 유저의 랭킹만 조회
Backend.URank.User.GetUserRank("rankUuid", "userIndate", callback => {
    // 이후 처리
});

// 위아래 3명의 유저를 포함하여 랭킹 조회
// 해당 유저가 4등인 경우 1 ~ 9등 유저가 조회됨
Backend.URank.User.GetUserRank("rankUuid", "userIndate", 5, callback => {
    // 이후 처리
});
```

### SendQueue
```js
// 해당 유저의 랭킹만 조회
SendQueue.Enqueue(Backend.URank.User.GetUserRank, "rankUuid", "userIndate", callback => {
    // 이후 처리
});

// 위아래 3명의 유저를 포함하여 랭킹 조회
// 랭킹에 총 10명의 유저가 있고, 해당 유저가 9등인 경우
// 6 ~ 10등 유저가 조회됨
SendQueue.Enqueue(Backend.URank.User.GetUserRank, "rankUuid", "userIndate", 3, callback => {
    // 이후 처리
});
```

## ReturnCase

### Success cases

**해당 유저가 랭킹에 존재하는 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**uuid가 null 혹은 string.Empty인 경우**  
statusCode : 400  
errorCode : ValidationException  
message : rankUuid is null or empty

**존재하지 않는 유저의 userIndate로 조회를 시도할 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer not found, gamer을(를) 찾을 수 없습니다

**존재하지 않는 uuid로 조회를 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : rank not found, rank을(를) 찾을 수 없습니다

**해당 유저가 랭킹에 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : userRank not found, userRank을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

### gap이 0인 경우
```js
{
    "rows": [
        {
            // 유저의 게이머 inDate
            "gamerInDate": {
                "S": "2021-03-11T06:37:30.934Z"
            },
            // 유저의 닉네임
            "nickname": {
                "S": "닉네임0호"
            },
            // 추가 항목
            // 컬럼명과 값이 그대로 노출됩니다.  
            // 추가 항목 컬럼명이 extraScore인 경우 extraScore
            "extraScore": {
                "N": "0"
            },
            // 점수
            // 컬럼명이 score로 통일됩니다.  
            // 랭킹 항목 컬럼명이 power인 경우에도 score
            "score": {
                "N": "101"
            },
            // offset
            "index": {
                "N": 98
            },
            // 유저의 랭킹
            "rank": {
                "N": 99
            }
        }
    ],
    // 랭킹에 등록된 총 랭커의 수
    "totalCount": 100
}
```

### gap이 1 인 경우
```js
{
    "rows": [
        // 자기 자신보다 랭킹이 1 높은 유저
        {
            "gamerInDate": {
                "S": "2021-03-11T06:37:32.616Z"
            },
            "nickname": {
                "S": "닉네임22호"
            },
            "extraScore": {
                "N": "2346"
            },
            "score": {
                "N": "102"
            },
            "index": {
                "N": 97
            },
            "rank": {
                "N": 98
            }
        },
        // 자기 자신
        {
            "gamerInDate": {
                "S": "2021-03-11T06:37:30.934Z"
            },
            "nickname": {
                "S": "닉네임0호"
            },
            "extraScore": {
                "N": "0"
            },
            "score": {
                "N": 101
            },
            "index": {
                "N": 98
            },
            "rank": {
                "N": 99
            }
        },
        // 자기 자신보다 랭킹이 1 낮은 유저
        {
            "gamerInDate": {
                "S": "2021-03-11T06:37:31.838Z"
            },
            "nickname": {
                "S": "닉네임44호"
            },
            "extraScore": {
                "N": "2323"
            },
            "score": {
                "N": "100"
            },
            "index": {
                "N": 99
            },
            "rank": {
                "N": 100
            }
        }
    ],
    "totalCount": 100
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
public void GetUserRankTest()
{
    string userUuid = "81272320-8c40-112b-b174-d9233f6bd0e8";
    string userIndate = "2022-03-02T01:51:32.967Z";

    List<RankItem> rankItemList = new List<RankItem>();

    BackendReturnObject bro = Backend.URank.User.GetUserRank(userUuid, userIndate, 3);

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
