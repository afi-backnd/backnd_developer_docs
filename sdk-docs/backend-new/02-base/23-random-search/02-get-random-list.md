---
sidebar_label: 랜덤 테이블 리스트 조회
description: "랜덤 테이블 리스트 조회"
---

# GetRandomPools

public Task< GetRandomPoolsResult > **GetRandomPoolsAsync**();

## 설명

뒤끝 콘솔에서 생성한 모든 랜덤 조회 내역을 불러옵니다.  
해당 리스트에는 다음 정보가 포함되어 있습니다.  

- 랜덤 조회명
- uuid
- 분류(user/guild)

## Example

### Task 방식

```js
var reqResult = await BackndRandom.Instance.GetRandomPoolsAsync();
```

### Callback 방식

```js
BackndRandom.Instance.GetRandomPools((callback) =>
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

**랜덤 조회가 0개인 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

## ReturnValueJson

```js
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

```js
public class RandomInfoTable
{
    public string title; // 랜덤 조회 제목
    public string uuid; // 랜덤 조회 uuid
    public RandomType randomtype; //랜덤 조회 유형

    public override string ToString() {
        return $"title: {title}\n" +
        $"uuid: {uuid}\n" +
        $"randomtype: {randomtype}";
    }
}
```

```js
public async Task GetRandomDataTableList()
{
    var reqResult = await BackndRandom.Instance.GetRandomPoolsAsync();
    if (reqResult.IsSuccess())
    {
        var infoList = reqResult.GetInfoList();
        List<RandomInfoTable> list = new List<RandomInfoTable>();

        for (int i = 0; i < infoList.Count; i++)
        {
            var info = infoList[i];
            RandomInfoTable table = new RandomInfoTable();

            table.title = info.Title;
            table.uuid = info.Uuid;
            table.randomtype = info.RandomType;

            list.Add(table);
        }

        foreach (var table in list)
        {
            Debug.Log(list.ToString());
        }
    }
}
```
