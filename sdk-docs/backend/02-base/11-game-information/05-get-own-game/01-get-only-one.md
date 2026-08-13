---
sidebar_label: "데이터 조회 (inDate이용)"
description: "GetMyData"
---

# GetMyData
public BackendReturnObject **GetMyData**(string **tableName**, string **inDate**);  
public BackendReturnObject **GetMyData**(string **tableName**, string **inDate**, string[] **select**);




## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| tableName   | string | 조회할 테이블명 | - |
| inDate   | string | 조회할 row의 inDate 값(owner_inDate값이 아닙니다.) | - |
| select | string[] | (Optional) 조회할 row 중 특정 컬럼만 보이게 하는 값 | 모든 컬럼을 포함 |



## 설명
자기 자신이 소유한 데이터 중 지정한 1개의 row를 조회합니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 조회할 수 있습니다.  
* public/private 여부 관계없이 자기 자신의 데이터만 조회할 수 있습니다.  
* select 절의 사용 유무와 관계없이 데이터의 처리량은 동일합니다.  

## Example

### 동기
```js
// select[]를 이용하여 리턴 시, owner_inDate와 score만 출력되도록 설정
string[] select = {"owner_inDate", "score"};

// 테이블 내 해당 rowIndate를 지닌 row를 조회
var bro = Backend.GameData.GetMyData("tableName", "rowIndate");
Debug.Log(bro);

// 테이블 내 해당 rowIndate를 지닌 row를 조회
// select에 존재하는 컬럼만 리턴
bro = Backend.GameData.GetMyData("tableName", "rowIndate", select);
Debug.Log(bro);
```

### 비동기
```js
// select[]를 이용하여 리턴 시, owner_inDate와 score만 출력되도록 설정
string[] select = {"owner_inDate", "score"};

// 테이블 내 해당 rowIndate를 지닌 row를 조회
Backend.GameData.GetMyData("tableName", "rowIndate", (callback) =>
{
    Debug.Log(callback);
});

// 테이블 내 해당 rowIndate를 지닌 row를 조회
// select에 존재하는 컬럼만 리턴
Backend.GameData.GetMyData("tableName", "rowIndate", select, (callback) =>
{
    Debug.Log(callback);
});
```

### SendQueue
```js
// 리턴 시, owner_inDate와 score만 출력되도록 설정
string[] select = {"owner_inDate", "score"};

// 테이블 내 해당 rowIndate를 지닌 row를 조회
SendQueue.Enqueue(Backend.GameData.GetMyData,"tableName", "rowIndate", (callback) =>
{
    Debug.Log(callback);
});

// 테이블 내 해당 rowIndate를 지닌 row를 조회
// select에 존재하는 컬럼만 리턴
SendQueue.Enqueue(Backend.GameData.GetMyData, "tableName", "rowIndate", select, (callback) =>
{
    Debug.Log(callback);
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**조회에 성공했으나 해당 inDate를 지닌 row가 존재하지 않는 경우**  
statusCode : 200  
message : Success  
returnValue : {"serverTime":"2021-03-23T07:04:50.114Z","row":{}}

### Error cases

**존재하지 않는 테이블에 Get을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : table not found, table을(를) 찾을 수 없습니다

**비활성화된 tableName에 불러오기를 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  



## GetReturnValuetoJSON
```js
{
    // 서버에서 요청을 처리한 서버 시간
    "serverTime":"2021-03-23T06:39:46.519Z",
    // 리턴된 데이터
    "row": {
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
        ... // 클라이언트에 저장된 데이터
    }
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

    public GameDataItem(LitJson.JsonData json)
    {
        hp = int.Parse(json["hp"].ToString());
        mp = int.Parse(json["mp"].ToString());
        atk = float.Parse(json["atk"].ToString());
        money = long.Parse(json["money"].ToString());

        foreach(var column in json["equip"].Keys)
        {
            equip.Add(column, json["equip"][column].ToString());
        }

        for(int i = 0; i < json["items"].Count; i++)
        {
            items.Add(json["items"][i].ToString());
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
public void GetInDate()
{
    var bro = Backend.GameData.GetMyData("PlayerInfo", "2022-03-15T07:16:44.031Z");

    if(!bro.IsSuccess())
    {
        Debug.LogError(bro.ToString());
        return;
    }

    LitJson.JsonData gameDataListJson = bro.GetFlattenJSON()["row"];

    if(gameDataListJson.Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }

    GameDataItem gameDataItem = new GameDataItem(gameDataListJson);
    Debug.Log(gameDataItem.ToString());
}
```
