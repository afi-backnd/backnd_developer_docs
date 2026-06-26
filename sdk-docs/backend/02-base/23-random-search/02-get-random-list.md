---
sidebar_label: 랜덤 테이블 리스트 조회
---

# GetRandomDataTableList

public BackendReturnObject **GetRandomDataTableList**();

## 설명

:::info 참고
랜덤 조회는 콘솔 설정에 따라 그룹별로 구분하여 운영할 수 있습니다.
그룹별 구분이 활성화된 랜덤 조회는 현재 유저(또는 길드)가 속한 그룹을 기준으로 동작하며, 그룹이 변경되면 RandomPool 데이터도 변경 된 그룹의 RandomPool로 이동합니다.  
:::

뒤끝 콘솔에서 생성한 랜덤 조회 목록을 조회합니다.  
조회 결과에는 각 랜덤 조회의 이름, uuid, 유형(user/guild) 정보가 포함됩니다.

## Example

### 동기

```csharp
Backend.RandomInfo.GetRandomDataTableList();
```

### 비동기

```csharp
Backend.RandomInfo.GetRandomDataTableList(callback =>
{
    // 이후 처리
});
```

### SendQueue

```csharp
SendQueue.Enqueue(
    Backend.RandomInfo.GetRandomDataTableList,
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

**랜덤 조회가 0개인 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

## GetReturnValuetoJSON

```json
{
    "rows": [
        {
            "randomType": "user",
            "uuid": "6f438bd0-01a6-11ed-bdc8-a700365a13a1",
            "title": "PVP용 랜덤 조회"
        },
        {
            "randomType": "user",
            "uuid": "94b07a80-00f3-11ed-ade3-6d5252f35aa5",
            "title": "친구추천용 랜덤 조회"
        },
        {
            "randomType": "guild",
            "uuid": "96ab48f0-fce9-11ec-a8e9-3fc17cd7d4bd",
            "title": "길드추천용 랜덤 조회"
        }
    ]
}
```

## Sample Code

```csharp
public class RandomInfoTable
{
    public string Title;
    public string Uuid;
    public RandomType RandomType;

    public override string ToString()
    {
        return $"title: {Title}\n" +
               $"uuid: {Uuid}\n" +
               $"randomType: {RandomType}";
    }
}
```

```csharp
public void GetRandomDataTableList()
{
    BackendReturnObject bro = Backend.RandomInfo.GetRandomDataTableList();

    if (bro.IsSuccess())
    {
        LitJson.JsonData rows = bro.Rows();
        List<RandomInfoTable> list = new List<RandomInfoTable>();

        for (int i = 0; i < rows.Count; i++)
        {
            RandomInfoTable table = new RandomInfoTable();
            table.Title = rows[i]["title"].ToString();
            table.Uuid = rows[i]["uuid"].ToString();
            table.RandomType = rows[i]["randomType"].ToString() == "user"
                ? RandomType.User
                : RandomType.Guild;

            list.Add(table);
        }

        foreach (RandomInfoTable table in list)
        {
            Debug.Log(table.ToString());
        }
    }
}
```
