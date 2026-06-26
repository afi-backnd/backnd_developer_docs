---
sidebar_label: 랜덤 그룹에서 데이터 조회
description: "랜덤 그룹에서 데이터 조회"
---

# PickRandomEntries

public Task< PickRandomEntriesResult > **PickRandomEntriesAsync**(RandomType **randomType**, string **uuid**, int **pivot**, int **gap**, int **count**);  
public Task< PickRandomEntriesResult > **PickRandomEntriesAsync**(RandomType **randomType**, string **uuid**, long **pivot**, int **gap**, int **count**);  
public Task< PickRandomEntriesResult > **PickRandomEntriesAsync**(RandomType **randomType**, string **uuid**, float **pivot**, int **gap**, int **count**);  
public Task< PickRandomEntriesResult > **PickRandomEntriesAsync**(RandomType **randomType**, string **uuid**, double **pivot**, int **gap**, int **count**);  

## 설명

해당 uuid에 존재하는 데이터들을 조건에 맞게 랜덤으로 불러옵니다.  

조건은 다음과 같습니다.  

- pivot : 중심이 되는 값
- gap : pivot에 일치하거나 제일 가까운 값의 순위를 기준으로 위아래 등수의 범위
- count : 불러올 개수

랜덤 조회에 등록된 점수가 10000, 1000, 100, 10, 1 순으로 되어있고 pivot이 100, gap이 1이면, 99~101점인 데이터를 랜덤 검색하는 것이 아니라, 100점이 속한 3등을 기준으로 2등(1000점)과 4등(10점) 사이에서 랜덤으로 검색합니다.  

만약 조건에 만족하는 데이터가 count만큼 존재하지 않을 경우, count보다 적은 데이터를 불러올 수 있습니다.  

gap은 최소 0에서 최대 10까지 입력할 수 있으며, 해당 범위를 넘어갈 경우 최소/최댓값이 재설정되어 검색됩니다
count는 최소 1에서 최대 10까지 입력할 수 있으며, 해당 범위를 넘어갈 경우 최소/최댓값이 재설정되어 검색됩니다.  
pivot은 long의 최댓값(9223372036854775807, 1.0E+17 미만)까지 입력할 수 있으며, 해당 값을 넘어갈 경우 최소/최댓값이 재설정되어 검색됩니다.  

닉네임은 제공되지 않으며, 유저/길드의 inDate만 제공됩니다.  

### 파라미터

| Value      | Type                  | Description                                                                 | min            | max           |
| ---------- | --------------------- | --------------------------------------------------------------------------- | -------------- | ------------- |
| randomType | RandomType            | 랜덤 조회 유형  
(RandomType.User는 유저, RandomType.Guild는 길드)        |                |               |
| uuid       | string                | 랜덤 조회의 uuid                                                            |                |               |
| pivot      | int/long/float/double | 중심이 되는 값                                                              | -long.MaxValue(음수) | long.MaxValue |
| gap        | int                   | pivot에 일치하거나 제일 가까운 값의 순위를 기준으로 위아래 등수의 범위          | 0              | 10            |
| count      | int                   | 불러올 개수                                                                 | 1              | 10            |

## Example

### Task 방식

```js
var reqRequest = BackndRandom.Instance.PickRandomEntriesAsync(RandomType.User, "7244eca0-f67c-11ec-9eb6-490af390b219", 20, 10, 5);
```

### Callback 방식

```js
BackndRandom.Instance.PickRandomEntries
    (RandomType.User, "7244eca0-f67c-11ec-9eb6-490af390b219", 20, 10, 5, (callback) =>
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

**조건에 맞는 유저/길드가 존재하지 않는 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

### Error cases

**입력한 uuid가 null 혹은 string.Empty일 경우**  
statusCode : 400  
errorCode : ValidationException  
message : uuid is null or empty

**랜덤 조회의 유형이 다를 경우(유저 유형인데 RandomType.Guild으로 지정했을 경우)**  
statusCode : 400  
errorCode : BadParameterException  
message : bad type is not match, 잘못된 type is not match 입니다

**존재하지 않는 uuid일 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : randomPool not found, randomPool을(를) 찾을 수 없습니다

## ReturnValueJson

### 랜덤유형(RandomType)이 User인 경우

```js
{
    "rows": [
        {
            "gamerInDate": "2022-06-30T10:06:32.327Z",
            "score": "13"
        },
        {
            "gamerInDate": "2022-06-30T10:06:34.024Z",
            "score": "19"
        },
        ...  
    ]
}
```

### 랜덤유형(RandomType)이 Guild인 경우

```js
{
    "rows": [
        {
            "guildName": "match7GG",
            "guildInDate": "2022-07-11T05:51:47.687Z",
            "score": "7"
        },
        {
            "guildName": "match12GG",
            "guildInDate": "2022-07-11T05:51:53.144Z",
            "score": "12"
        },
        ...  
    ]
}
```

## Sample Code

```js
public class RandomInformation
{

    public string inDate; // inDate
    public int score; // 점수
    public string guildName; //(길드만 해당)길드이름

    public override string ToString() {
        string str = $"inDate: {inDate}\n" +
        $"score: {score}";
        if(!string.IsNullOrEmpty(guildName)) {
            str += $"\nguildName : {guildName}";
        }
        return str;
    }
}
```

```js
public async Task GetRandomData()
{
    RandomType type = RandomType.Guild;
    var reqResult = await BackndRandom.Instance.PickRandomEntriesAsync(type, "96ab48f0-fce9-11ec-a8e9-3fc17cd7d4bd", 10, 10, 10);
    if (reqResult.IsSuccess())
    {
        List<RandomInformation> list = new List<RandomInformation>();
        var infoList = reqResult.GetInfoList();
        for (int i = 0; i < infoList.Count; i++)
        {
            var info = infoList[i];

            RandomInformation randomInformation = new RandomInformation();               
            randomInformation.inDate = info.InDate;
            randomInformation.guildName = info.GuildName;
            randomInformation.score = int.Parse(info.Score);

            list.Add(randomInformation);
        }
        foreach (var info in list)
        {
            Debug.Log(info.ToString());
        }
    }
    else
    {
        Debug.LogError("랜덤 조회 에러 : " + reqResult.ToString());
    }
}
```
