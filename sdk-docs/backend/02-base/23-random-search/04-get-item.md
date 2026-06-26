---
sidebar_label: 랜덤 그룹에서 데이터 조회
---

# GetRandomData

public BackendReturnObject **GetRandomData**(RandomType **randomType**, string **uuid**, int **pivot**, int **gap**, int **count**);  
public BackendReturnObject **GetRandomData**(RandomType **randomType**, string **uuid**, long **pivot**, int **gap**, int **count**);  
public BackendReturnObject **GetRandomData**(RandomType **randomType**, string **uuid**, float **pivot**, int **gap**, int **count**);  
public BackendReturnObject **GetRandomData**(RandomType **randomType**, string **uuid**, double **pivot**, int **gap**, int **count**);

## 설명

해당 uuid에 등록된 데이터 중 조건에 맞는 대상을 랜덤으로 조회합니다.  

조회 조건은 다음과 같습니다.  

- pivot : 기준 값
- gap : pivot과 일치하거나 가장 가까운 값의 순위를 기준으로 탐색할 범위
- count : 조회할 데이터 개수

이 함수는 점수 범위가 아니라 순위를 기준으로 대상을 탐색합니다. 예를 들어 등록된 점수가 10000, 1000, 100, 10, 1 순이고 pivot이 100, gap이 1이면, 99~101점 범위를 찾는 것이 아니라 100점이 위치한 순위를 기준으로 앞뒤 한 단계 범위를 조회합니다. 이 경우 조회 대상은 2등(1000점)부터 4등(10점)까지입니다.  

- 조건에 맞는 데이터 수가 count보다 적은 경우, 존재하는 데이터만 반환됩니다.
- gap은 0~10 범위로 보정되어 처리되며, count는 1~10 범위로 보정되어 처리됩니다.
- pivot은 int, long, float, double 타입을 사용할 수 있습니다.
- 조회 결과에는 닉네임이 포함되지 않으며, 유저/길드의 inDate가 제공됩니다.

### 파라미터

| Value      | Type                  | Description                                                             | min  | max |
| ---------- | --------------------- | ----------------------------------------------------------------------- | ---- | --- |
| randomType | RandomType            | 랜덤 조회 유형(RandomType.User는 유저, RandomType.Guild는 길드)         |      |     |
| uuid       | string                | 랜덤 조회 uuid                                                          |      |     |
| pivot      | int/long/float/double | 기준 값                                                                 |      |     |
| gap        | int                   | pivot과 일치하거나 가장 가까운 값의 순위를 기준으로 탐색할 범위         | 0    | 10  |
| count      | int                   | 조회할 데이터 개수                                                      | 1    | 10  |

## Example

### 동기

```csharp
Backend.RandomInfo.GetRandomData(RandomType.User, "7244eca0-f67c-11ec-9eb6-490af390b219", 20, 10, 5);
```

### 비동기

```csharp
Backend.RandomInfo.GetRandomData(
    RandomType.User,
    "7244eca0-f67c-11ec-9eb6-490af390b219",
    20,
    10,
    5,
    callback =>
    {
        // 이후 처리
    });
```

### SendQueue

```csharp
SendQueue.Enqueue(
    Backend.RandomInfo.GetRandomData,
    RandomType.User,
    "7244eca0-f67c-11ec-9eb6-490af390b219",
    20,
    10,
    5,
    callback =>
    {
        // 이후 처리
    });
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

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

## GetReturnValuetoJSON

### 랜덤유형(RandomType)이 User인 경우

```json
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

```json
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

```csharp
public class RandomInformation
{
    public string InDate;
    public int Score;
    public string GuildName;

    public override string ToString()
    {
        string str = $"inDate: {InDate}\n" +
                     $"score: {Score}";

        if (!string.IsNullOrEmpty(GuildName))
        {
            str += $"\nguildName: {GuildName}";
        }

        return str;
    }
}
```

```csharp
public void GetRandomData()
{
    RandomType type = RandomType.Guild;
    BackendReturnObject bro = Backend.RandomInfo.GetRandomData(type, "96ab48f0-fce9-11ec-a8e9-3fc17cd7d4bd", 10, 10, 10);

    if (bro.IsSuccess())
    {
        List<RandomInformation> list = new List<RandomInformation>();
        LitJson.JsonData rows = bro.Rows();

        for (int i = 0; i < rows.Count; i++)
        {
            RandomInformation randomInformation = new RandomInformation();

            if (type == RandomType.User)
            {
                randomInformation.InDate = rows[i]["gamerInDate"].ToString();
            }
            else
            {
                randomInformation.GuildName = rows[i]["guildName"].ToString();
                randomInformation.InDate = rows[i]["guildInDate"].ToString();
            }

            randomInformation.Score = int.Parse(rows[i]["score"].ToString());
            list.Add(randomInformation);
        }

        foreach (RandomInformation info in list)
        {
            Debug.Log(info.ToString());
        }
    }
    else
    {
        Debug.LogError("랜덤 조회 에러 : " + bro);
    }
}
```
