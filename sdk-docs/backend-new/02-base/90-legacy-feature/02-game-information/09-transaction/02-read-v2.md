---
sidebar_label: 트랜잭션 읽기 V2
description: "트랜잭션 읽기 V2"
---

# TransactionReadV2
public Task&lt;RequestResult&gt; **TransactionReadV2Async**(TransactionValue[\] **transactionList**);  
public Task&lt;RequestResult&gt; **TransactionReadV2Async**(List<TransactionValue\> **transactionList**);

## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| transactionList | [TransactionValue\[\] ](/sdk-docs/backend/base/game-information/transaction/crud/classes) | (selection) 트랜잭션 작업들을 배열 | 
| transactionList | List<TransactionValue\> | (selection) 트랜잭션 작업들을 저장한 리스트 | 


## 설명
transactionList에 존재하는 트랜잭션 읽기 명령들을 서버에 요청합니다.  
* 최대 10개의 작업을 1개로 묶을 수 있습니다.  
* **inDate**를 이용한 데이터 검색의 경우, SetGet 대신 **SetGetV2**의 작업만 사용할 수 있습니다.  
* 작업 리스트에 존재하는 요청들 중 하나 이상의 작업에서 에러가 발생할 경우 모든 요청들은 실패 처리가 됩니다.  
* 모든 요청이 성공해야 모든 요청들이 성공 처리가 됩니다.  
* where 조건으로 다수의 데이터가 검색될 경우, 제일 최근에 삽입된 데이터 1개만 리턴됩니다.  
* 서로 다른 where조건일지라도 검색되는 데이터가 일치한다면 에러가 발생할 수 있습니다.  

## Example

### List< TransactionValue >
```js
List<TransactionValue> transactionList = new List<TransactionValue>();

//자신의 데이터일 경우
transactionList.Add(TransactionValue.SetGetV2("tableName", "row_indate", BackndBase.UserInDate));

//타인의 데이터일 경우
transactionList.Add(TransactionValue.SetGetV2("tableName", "row_indate", "owner_inDate"));

Where where = new Where();
where.Equal("nickname","뒤끝이");
transactionList.Add(TransactionValue.SetGet("tableName", where));

Where where2 = new Where();
where2.Less("level",10);
where2.Contains("guildName","뒤끝모험길드단");
transactionList.Add(TransactionValue.SetGet("tableName", where2));

```

### Task 형식
```js
var reqResult = await BackndLegacy.GameData.TransactionReadV2Async(transactionList);  
```

### Callback 형식
```js
BackndLegacy.GameData.TransactionReadV2(transactionList, (callback) => 
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**모든 요청이 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**트랜잭션 작업을 수행 중 1개 이상의 작업에서 에러가 발생한 경우**  
에러 상황에 대응하는 에러가 리턴됩니다.  
Get 을 수행 시 에러에 대해서는 개발자 문서를 참고해 주세요.  

**동일한 row에 2개 이상의 작업을 수행했을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Transaction request cannot include multiple operations on one item

**transactionList에 10개를 초과하는 작업이 있거나 혹은 작업이 존재하지 않는 경우**  
statusCode : 400  
errorCode : TransactionSizeError  
message : Not Support Transaction Size: {transactionList에 존재하는 작업 개수}

**transactionList에 Get 명령 이외의 작업이 존재하는 경우**  
statusCode : 400  
errorCode : TransactionDataError  
message : Not Support Type in TransactionRead: {에러가 발생한 명령}

**transactionList에 inDate를 이용한 SetGet의 작업이 존재하는 경우(SetGetV2를 사용해야합니다.)**  
statusCode : 400  
errorCode : TransactionDataError  
message : V2 must use indate with owner_inDate: {에러가 발생한 명령}

## GetReturnValuetoJSON
```js
{
    // 리턴된 데이터
    "Responses":[
         // 첫번째로 transactionList.Add한 데이터
        {
            // 해당 row를 소유한 유저의 inDate
            "owner_inDate":{
                "S":"2019-07-31T06:15:35.691Z"
            },
            // 해당 row를 insert 한 시간
            "client_date":{
                "S":"2021-03-23T04:54:45.709Z"
            },
            // 해당 row의 inDate
            "inDate":{
                "S":"2021-03-23T04:54:45.849Z"
            },
            // 해당 row가 마지막으로 업데이트된 시간
            "updatedAt":{
                "S":"2021-03-23T04:54:45.849Z"
            },
            "score":{
                "N":"1234"
            },
            "item1":{
                "S":"롱소드"
            },
            ... // 클라이언트에 저장된 데이터
        },
         // 두번째로 transactionList.Add한 데이터
        {
            // 해당 row를 소유한 유저의 inDate
            "owner_inDate":{
                "S":"2019-03-21T01:15:35.621Z"
            },
            // 해당 row를 insert 한 시간
            "client_date":{
                "S":"2021-03-13T02:54:45.709Z"
            },
            // 해당 row의 inDate
            "inDate":{
                "S":"2021-03-13T04:54:45.849Z"
            },
            // 해당 row가 마지막으로 업데이트된 시간
            "updatedAt":{
                "S":"2021-03-23T04:54:45.849Z"
            },
            "item1":{
                "S":"뒤끝이의 검"
            },
            ... // 클라이언트에 저장된 데이터
        },
        // and etc...  
     }
    ]
}
```

## Sample Code
```js
public class GameDataItem
{
    public string nickName = Backend.UserNickName;
    public string ownerIndate = Backend.UserInDate;
    public string inDate;
    public int hp;
    public int mp;
    public float atk;
    public long money;
    public Dictionary<string, string> equip = new Dictionary<string, string>();
    public List<string> items = new List<string>();
    public DateTime lastUpdate;

    public GameDataItem()
    {
    }

    public GameDataItem(JObject json)
    {
        hp = int.Parse(json["hp"].ToString());
        mp = int.Parse(json["mp"].ToString());
        atk = float.Parse(json["atk"].ToString());
        money = long.Parse(json["money"].ToString());

        var equipObj = json["equip"] as JObject;
        foreach (var property in equipObj.Properties())
        {
            equip.Add(property.Name, property.Value.ToString());
        }

        var itemArray = json["items"] as JArray;
        for (int i = 0; i < itemArray.Count; i++)
        {
            items.Add(itemArray[i].ToString());
        }
        inDate = json["inDate"].ToString();
        lastUpdate = DateTime.Parse(json["lastUpdate"].ToString());
    }

    public Param ToParam()
    {
        Param param = new Param();

        param.Add("nickName", nickName);
        param.Add("hp", hp);
        param.Add("mp", mp);
        param.Add("atk", atk);
        param.Add("money", money);
        param.Add("equip", equip);
        param.Add("items", items);
        param.Add("lastUpdate", DateTime.UtcNow);

        return param;
    }
    public override string ToString()
    {
        string equipString = "equip\n";
        foreach(var dic in equip)
        {
            equipString += $"-{dic.Key} : {dic.Value}\n";
        }

        string itemString = "items : ";
        for(int i = 0; i < items.Count; i++)
        {
            itemString += $"{items[i]}, ";
        }

        return $"hp : {hp}\n" +
         $"mp : {mp}\n" +
         $"atk : {atk}\n" +
         $"money : {money}\n" +
         $"lastUpdate : {lastUpdate}\n" +
         equipString + "\n" + itemString + "\n";
    }
}
```

```js
public async Task TransactionReadV2()
{
    List<TransactionValue> transactionList = new List<TransactionValue>();
    transactionList.Add(TransactionValue.SetGetV2("PlayerInfo", "2022-03-15T07:16:44.131Z", "2021-11-16T06:57:55.552Z"));
    transactionList.Add(TransactionValue.SetGet("PlayerInfo", new Where()));

    var reqResult = await BackndLegacy.GameData.TransactionReadV2Async(transactionList);
    if (!reqResult.IsSuccess())
    {
        Debug.LogError(reqResult.ToString());
        return;
    }

    var gameDataListJson = reqResult.GetRows("Responses");
    if (gameDataListJson.Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }

    for (int i = 0; i < gameDataListJson.Count; i++)
    {
        GameDataItem gameDataItem = new GameDataItem(gameDataListJson[i] as JObject);
        Debug.Log(gameDataItem.ToString());
    }
}
```
