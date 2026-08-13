---
sidebar_label: "트랜잭션 읽기 (데이터 읽기)"
description: "TransactionRead"
---

# TransactionRead
public BackendReturnObject **TransactionRead**(PlayerDataTransactionRead **playerDataTransactionRead**)

## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| playerDataTransactionRead   | PlayerDataTransactionRead | 읽기 전용 트랜잭션 명령 모음 클래스 | - |

### PlayerDataTransactionRead
데이터 불러오기 명령들을 추가하는 클래스입니다.

**전체 데이터중 최신 데이터 하나 불러오기**  
public PlayerDataTransactionRead **AddGetAllLatestData**(string **tableName**);  

```js
PlayerDataTransactionRead playerDataTransactionRead = new PlayerDataTransactionRead();

// 해당 테이블에서 가장 최근에 삽입된 데이터
playerDataTransactionRead.AddGetAllLatestData("SCORE");
```

**자기 자신의 데이터 불러오기**  
public PlayerDataTransactionRead **AddGetMyLatestData**(string **tableName**);  
public PlayerDataTransactionRead **AddGetMyData**(string **tableName**, string **inDate**);  

```js
PlayerDataTransactionRead playerDataTransactionRead = new PlayerDataTransactionRead();

// 해당 테이블에서 가장 최근에 자기 자신이 등록한 데이터
playerDataTransactionRead.AddGetMyLatestData("SCORE");

// 내가 등록한 데이터 중 inDate가 "2023-09-14T12:43:54.451Z"인 데이터 불러오기 
playerDataTransactionRead.AddGetMyData("USER_DATA", "2023-09-14T12:43:54.451Z");
```

**타인의 데이터 불러오기**  
public PlayerDataTransactionRead **AddGetOtherLatestData**(string **tableName**, string **owner_inDate**);  
public PlayerDataTransactionRead **AddGetOtherData**(string **tableName**, string **inDate**, string **owner_inDate**);  

```js
// 유저 이름을 검색하여 해당 유저의 owner_inDate 알아내기
var userBro = Backend.Social.GetUserInfoByNickName("nickName");
string otherOwnerIndate = userBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

PlayerDataTransactionRead playerDataTransactionRead = new PlayerDataTransactionRead();

// 해당 테이블에서 owner_inDate가 일치하는 유저가 등록한 가장 최근 데이터 불러오기
playerDataTransactionRead.AddGetOtherLatestData("SCORE", otherOwnerIndate);

// 해당 테이블에서 owner_inDate가 일치하는 유저가 등록한 데이터 중 inDate가 "2023-09-14T12:43:54.451Z"인 데이터 불러오기
playerDataTransactionRead.AddGetOtherData("USER_DATA", "2023-09-14T12:43:54.451Z", otherOwnerIndate);
```

## 설명
transactionList에 존재하는 트랜잭션 읽기 명령들을 서버에 요청합니다.    
* 조건이 일치하는 데이터가 없을 경우에는 rows[]가 아닌 에러가 발생합니다.
* 최대 10개의 작업을 1개로 묶을 수 있습니다.    
* 작업 리스트에 존재하는 요청들 중 하나 이상의 작업에서 에러가 발생할 경우 모든 요청들은 실패 처리가 됩니다.  
* 모든 요청이 성공해야 모든 요청들이 성공 처리가 됩니다.  
* playerDataTransactionRead 내 명령 중 동일한 row을 불러올 경우, 에러가 발생합니다.  


## Example

### 동기
```js
PlayerDataTransactionRead playerDataTransactionRead = new PlayerDataTransactionRead();

playerDataTransactionRead.AddGetAllLatestData("PUBLIC_DATA");

playerDataTransactionRead.AddGetMyLatestData("ACHIVE_DATA");
playerDataTransactionRead.AddGetMyData("USER_DATA", "2023-09-17T14:16:32.341Z");

var userBro = Backend.Social.GetUserInfoByNickName("nickName");
string otherOwnerIndate = userBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

playerDataTransactionRead.AddGetOtherLatestData("SCORE", otherOwnerIndate);
playerDataTransactionRead.AddGetOtherData("USER_DATA", "2023-09-19T16:21:12.542Z", otherOwnerIndate);

Backend.PlayerData.TransactionRead(playerDataTransactionRead);
```

### 비동기
```js
PlayerDataTransactionRead playerDataTransactionRead = new PlayerDataTransactionRead();

playerDataTransactionRead.AddGetAllLatestData("PUBLIC_DATA");

playerDataTransactionRead.AddGetMyLatestData("ACHIVE_DATA");
playerDataTransactionRead.AddGetMyData("USER_DATA", "2023-09-17T14:16:32.341Z");

var userBro = Backend.Social.GetUserInfoByNickName("nickName");
string otherOwnerIndate = userBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

playerDataTransactionRead.AddGetOtherLatestData("SCORE", otherOwnerIndate);
playerDataTransactionRead.AddGetOtherData("USER_DATA", "2023-09-19T16:21:12.542Z", otherOwnerIndate);

Backend.PlayerData.TransactionRead(playerDataTransactionRead, callback => {

});
```

### SendQueue
```js
PlayerDataTransactionRead playerDataTransactionRead = new PlayerDataTransactionRead();

playerDataTransactionRead.AddGetAllLatestData("PUBLIC_DATA");

playerDataTransactionRead.AddGetMyLatestData("ACHIVE_DATA");
playerDataTransactionRead.AddGetMyData("USER_DATA", "2023-09-17T14:16:32.341Z");

var userBro = Backend.Social.GetUserInfoByNickName("nickName");
string otherOwnerIndate = userBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

playerDataTransactionRead.AddGetOtherLatestData("SCORE", otherOwnerIndate);
playerDataTransactionRead.AddGetOtherData("USER_DATA", "2023-09-19T16:21:12.542Z", otherOwnerIndate);

Sendqueue.Enqueue(Backend.PlayerData.TransactionRead, playerDataTransactionRead, callback => {
    
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

**10개를 초과하는 작업이 있거나 혹은 작업이 존재하지 않는 경우**  
statusCode : 400  
errorCode : TransactionSizeError  
message : Not Support Transaction Size: {transactionList에 존재하는 작업 개수}

**조건에 일치하지 않아 데이터를 찾지 못한 요청이 존재할 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gameInfo not found, gameInfo을(를) 찾을 수 없습니다

## GetReturnValuetoJSON
```js
{
    "Responses": [
        {
            "stringData": {
                "S": "DWW96NRXHI2F"
            },
            "floatData": {
                "N": "0.22267"
            },
            "dateTimeData": {
                "S": "2023-10-30 16:01:11.250"
            },
            "boolData": {
                "BOOL": true
            },
            "doubleData": {
                "N": "0.541407"
            },
            "inDate": {
                "S": "2023-10-30T16:01:11.624Z"
            },
            "updatedAt": {
                "S": "2023-10-30T16:01:11.624Z"
            },
            "dicData": {
                "M": {
                    "key1": {
                        "S": "RO3G4CSUSSYP"
                    },
                    "key2": {
                        "S": "XZ48X72RW26H"
                    },
                    "key3": {
                        "S": "7HWQXXSQW9FT"
                    }
                }
            },
            "owner_inDate": {
                "S": "2022-07-18T07:42:46.490Z"
            },
            "listData": {
                "L": [
                    {
                        "S": "7OICP9Z8GSGI"
                    },
                    {
                        "S": "1HMC1VSIIX02"
                    }
                ]
            },
            "intData": {
                "N": "856602"
            }
        }
    ],
    "ConsumedCapacity": [
        {
            "Read": {
                "CapacityUnits": 2.5
            }
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
public void PlayerDataTransactionReadTest()
{
    PlayerDataTransactionRead playerDataTransactionRead = new PlayerDataTransactionRead();

    playerDataTransactionRead.AddGetAllLatestData("USER_DATA");

    playerDataTransactionRead.AddGetMyData("USER_DATA", "2023-09-17T14:16:32.341Z");

    var bro = Backend.PlayerData.TransactionRead(playerDataTransactionRead);

    if(!bro.IsSuccess())
    {
        Debug.LogError(bro.ToString());
        return;
    }

    LitJson.JsonData gameDataListJson = bro.GetFlattenJSON()["Responses"];

    if(gameDataListJson.Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }

    for(int i = 0; i < gameDataListJson.Count; i++)
    {
        GameDataItem gameDataItem = new GameDataItem(gameDataListJson[i]);
        Debug.Log(gameDataItem.ToString());
    }
}
```
