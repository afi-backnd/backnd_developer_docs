---
sidebar_label: "타인의 데이터 불러오기"
description: "GetOtherData"
---

# GetOtherData
public BackendReturnObject **GetOtherData**(string **tableName**, string **inDate**, string **owner_inDate**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **inDate**, string **owner_inDate**, string[] **select**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **owner_inDate**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **owner_inDate**, int **limit**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **owner_inDate**, int **limit**, string **firstKey**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **owner_inDate**, int **limit**, string **firstKey**, TableSortOrder **tableSortOrder**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **owner_inDate**, string[] **select**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **owner_inDate**, string[] **select**, int **limit**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **owner_inDate**, string[] **select**, int **limit**, string **firstKey**);  
public BackendReturnObject **GetOtherData**(string **tableName**, string **owner_inDate**, string[] **select**, int **limit**, string **firstKey**, TableSortOrder **tableSortOrder**);  


## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| tableName   | string | 조회할 테이블명 | - |
| inDate | string | 특정 row(데이터)의 고유 ID | - |
| owner_inDate | string | 유저의 고유 ID | - |
| limit | int | (Optional) 불러올 게임 정보 row 수. 최소 1, 최대 100. | 10 |
| select | string[] | (Optional) row 내 존재하는 컬럼 중 포함 시키고자 하는 컬럼 | 모든 컬럼을 포함 |
| [firstKey](/sdk-docs/backend/base/knowhow/object-for-return/paging/basic) | string | (Optional) 데이터를 조회하기 위한 시작점 | 제일 마지막에 insert 된 데이터 |

## 설명
테이블에 저장되어 있는 값 중 owner_inDate와 일치하는 유저의 데이터를 limit 수만큼 불러옵니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 조회할 수 있습니다.  
* private 테이블에서는 타인의 데이터를 조회 할 수 없습니다.  
* owner_inDate에 타인의 owner_inDate(Backend.UserInDate)를 입력할 경우, 타인의 데이터를 불러옵니다.
* select 절의 사용 유무와 관계없이 데이터의 처리량은 동일합니다.  

## Example

### 동기
```js
// 유저 이름을 검색하여 해당 유저의 owner_inDate 알아내기
var userBro = Backend.Social.GetUserInfoByNickName("nickName");
string otherOwnerIndate = userBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

 BackendReturnObject bro = null;
 
 // tableName에서 최대 10개의 타인이 등록한 row 불러오기
 bro = Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate);
 // 불러오기에 실패할 경우
 if(bro.IsSuccess() == false) 
 {
    Debug.Log("데이터 읽기 중에 문제가 발생했습니다 : " + bro.ToString());
 }
 // 불러오기에는 성공했으나 데이터가 존재하지 않는 경우
 if(bro.IsSuccess() && bro.FlattenRows().Count <= 0) 
 {
    Debug.Log("데이터가 존재하지 않습니다");
 }
 // 1개 이상 데이터를 불러온 경우
 if(bro.FlattenRows().Count > 0) 
 {
    string inDate = bro.FlattenRows()[0]["inDate"].ToString();
    int level = int.Parse(bro.FlattenRows()[0]["level"].ToString());
 }
 // tableName에서 최대 1개의 타인이 등록한 row 불러오기
 bro = Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, 1);

//  위에서 호출한 최대 1개의 타인이 등록한 row 불러오기 이후, 그 아래로 더 불러올 오래된 데이터가 있는지 확인
if(bro.HasFirstKey() == false) 
{
    Debug.Log("불러올 데이터가 존재하지 않습니다");
}

// 위에서 호출한 최대 1개의 타인이 등록한 row 불러오기 이후의 데이터가 존재할 경우, 그 이후의 데이터 불러오기
// (최신순 -> 오래된 순으로 데이터가 불러와집니다.)
 bro = Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, 1, bro.FirstKeystring());

 bro = Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, 1, bro.FirstKeystring(), TableSortOrder.DESC);

// 데이터를 불러올 때 selectList에 포함된 컬럼만 보여줍니다.
 string[] selectList = new string[] { "intData", "doubleData" };

 // tableName에서 최대 10개의 타인이 등록한 row 불러오기(selectList에 표시된 컬럼 외에는 표시되지 않습니다.)
 bro = Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, selectList);

 // tableName에서 최대 1 타인이 등록한 row 불러오기(selectList에 표시된 컬럼 외에는 표시되지 않습니다.)
 bro = Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, selectList, 1);

// 위에서 호출한 최대 1개의 타인이 등록한 row 불러오기 이후의 데이터가 존재할 경우, 그 이후의 데이터 불러오기
// (최신순 -> 오래된 순으로 데이터가 불러와집니다.)
// (selectList에 표시된 컬럼 외에는 표시되지 않습니다.)
 bro = Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, selectList, 1, bro.FirstKeystring());

 bro = Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, selectList, 1, bro.FirstKeystring(), TableSortOrder.DESC);

// 위에서 호출한 불러오기의 값 중, inDate가 일치하는 데이터를 불러오기
 bro = Backend.PlayerData.GetOtherData(tableName, bro.FlattenRows()[0]["owner_inDate"].ToString(), bro.FlattenRows()[0]["inDate"].ToString());

// 위에서 호출한 불러오기의 값 중, inDate가 일치하는 데이터를 불러오기(selectList에 표시된 컬럼 외에는 표시되지 않습니다.)
 bro = Backend.PlayerData.GetOtherData(tableName, bro.FlattenRows()[0]["owner_inDate"].ToString(), bro.FlattenRows()[0]["inDate"].ToString(), selectList);

```

### 비동기
```js
// 유저 이름을 검색하여 해당 유저의 owner_inDate 알아내기
var userBro = Backend.Social.GetUserInfoByNickName("nickName");
string otherOwnerIndate = userBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

string[] selectList = new string[] { "intData", "doubleData" };

Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, callback =>
{
    if(callback.IsSuccess() == false) 
    {
       Debug.Log("데이터 읽기 중에 문제가 발생했습니다 : " + callback.ToString());
    }
    // 불러오기에는 성공했으나 데이터가 존재하지 않는 경우
    if(callback.IsSuccess() && callback.FlattenRows().Count <= 0) 
    {
       Debug.Log("데이터가 존재하지 않습니다");
    }
    // 1개 이상 데이터를 불러온 경우
    if(callback.FlattenRows().Count > 0) 
    {
       string inDate = callback.FlattenRows()[0]["inDate"].ToString();
       int level = int.Parse(callback.FlattenRows()[0]["level"].ToString());
    }
});

Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, 1, callback =>
{
    if (callback.IsSuccess() && callback.FlattenRows().Count <= 0) 
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }

    if(callback.HasFirstKey() == false) 
    {
        Debug.Log("이후 읽을 데이터가 존재하지 않습니다");
        return;
    }
    Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, 1, callback.FirstKeystring(), callback2 =>
    {
        // callback2 처리
    });
    Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, 1, callback.FirstKeystring(), TableSortOrder.DESC, callback2 =>
    {
        // callback2 처리
    });
});


Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, selectList, callback =>
{
    if (callback.IsSuccess() && callback.FlattenRows().Count <= 0) 
    {
        Debug.Log("데이터가 존재하지 않습니다");
    }
});

Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, selectList, 1, callback =>
{
    if (callback.IsSuccess() && callback.FlattenRows().Count <= 0) 
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }
    if(callback.HasFirstKey() == false) 
    {
        Debug.Log("이후 읽을 데이터가 존재하지 않습니다");
        return;
    }

    Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, selectList, 1, callback.FirstKeystring(), callback2 =>
    {
        // callback2 처리
    });
    Backend.PlayerData.GetOtherData(tableName, otherOwnerIndate, selectList, 1, callback.FirstKeystring(), TableSortOrder.DESC, callback2 =>
    {
        // callback2 처리
    });
});

// 위에서 호출한 불러오기의 값 중, inDate가 일치하는 데이터를 불러오기
Backend.PlayerData.GetOtherData(tableName, bro.FlattenRows()[0]["owner_inDate"].ToString(), bro.FlattenRows()[0]["inDate"].ToString(), callback =>
{

});
// 위에서 호출한 불러오기의 값 중, inDate가 일치하는 데이터를 불러오기(selectList에 표시된 컬럼 외에는 표시되지 않습니다.)
Backend.PlayerData.GetOtherData(tableName, bro.FlattenRows()[0]["owner_inDate"].ToString(), bro.FlattenRows()[0]["inDate"].ToString(), selectList, callback =>
{

});
```

### SendQueue
```js
// 유저 이름을 검색하여 해당 유저의 owner_inDate 알아내기
var userBro = Backend.Social.GetUserInfoByNickName("nickName");
string otherOwnerIndate = userBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

string[] selectList = new string[] { "intData", "doubleData" };
 
SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, otherOwnerInDate, callback =>
{
    if(callback.IsSuccess() == false) 
    {
       Debug.Log("데이터 읽기 중에 문제가 발생했습니다 : " + callback.ToString());
    }
    // 불러오기에는 성공했으나 데이터가 존재하지 않는 경우
    if(callback.IsSuccess() && callback.FlattenRows().Count <= 0) 
    {
       Debug.Log("데이터가 존재하지 않습니다");
    }
    // 1개 이상 데이터를 불러온 경우
    if(callback.FlattenRows().Count > 0) 
    {
       string inDate = callback.FlattenRows()[0]["inDate"].ToString();
       int level = int.Parse(callback.FlattenRows()[0]["level"].ToString());
    }
});

SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, otherOwnerInDate, 1, callback =>
{
    if (callback.IsSuccess() && callback.FlattenRows().Count <= 0) 
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }

    if(callback.HasFirstKey() == false) 
    {
        Debug.Log("이후 읽을 데이터가 존재하지 않습니다");
        return;
    }
    SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, otherOwnerInDate, 1, callback.FirstKeystring(), callback2 =>
    {
        // callback2 처리
    });
    SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, otherOwnerInDate, 1, callback.FirstKeystring(), TableSortOrder.DESC, callback2 =>
    {
        // callback2 처리
    });
});


SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, otherOwnerInDate, selectList, callback =>
{
    if (callback.IsSuccess() && callback.FlattenRows().Count <= 0) 
    {
        Debug.Log("데이터가 존재하지 않습니다");
    }
});

SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, otherOwnerInDate, selectList, 1, callback =>
{
    if (callback.IsSuccess() && callback.FlattenRows().Count <= 0) 
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }
    if(callback.HasFirstKey() == false) 
    {
        Debug.Log("이후 읽을 데이터가 존재하지 않습니다");
        return;
    }

    SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, otherOwnerInDate, selectList, 1, callback.FirstKeystring(), callback2 =>
    {
        // callback2 처리
    });
    SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, otherOwnerInDate, selectList, 1, callback.FirstKeystring(), TableSortOrder.DESC, callback2 =>
    {
        // callback2 처리
    });
});

// 위에서 호출한 불러오기의 값 중, inDate가 일치하는 데이터를 불러오기
SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, bro.FlattenRows()[0]["owner_inDate"].ToString(), bro.FlattenRows()[0]["inDate"].ToString(), callback =>
{

});
// 위에서 호출한 불러오기의 값 중, inDate가 일치하는 데이터를 불러오기(selectList에 표시된 컬럼 외에는 표시되지 않습니다.)
SendQueue.Enqueue(Backend.PlayerData.GetOtherData, tableName, bro.FlattenRows()[0]["owner_inDate"].ToString(), bro.FlattenRows()[0]["inDate"].ToString(), selectList, callback =>
{

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
returnValue : {"serverTime":"2023-10-29T13:46:51.305Z","rows":[],"ConsumedCapacity":{"Read":{"CapacityUnits":0.5}}}

### Error cases
**존재하지 않는 유저의 owner_inDate일 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad owner_inDate, 잘못된 owner_inDate 입니다  

**private 테이블에서 타인의 owner_inDate로 데이터를 조회할 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad owner_inDate, 잘못된 owner_inDate 입니다  

**inDate를 이용한 GetOtherData 시, inDate가 string.Empty일 경우**  
statusCode : 400  
errorCode : ClientException  
message : inDate is empty  

**존재하지 않는 테이블에 Get을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : {테이블명} table not found, {테이블명} table을(를) 찾을 수 없습니다

**비활성화된 tableName에 불러오기를 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  

## GetReturnValuetoJSON(rows 내부에 존재하는 값은 모두 데이터에 따라 다릅니다.)
```js
{
    "serverTime": "2023-10-29T13:09:53.832Z",
    "rows": [
        {
            "stringData": {
                "S": "57J5EO3CJOGE"
            },
            "floatData": {
                "N": "0.17887"
            },
            "dateTimeData": {
                "S": "2023-10-29 13:09:53.401"
            },
            "boolData": {
                "BOOL": true
            },
            "doubleData": {
                "N": "0.139331"
            },
            "inDate": {
                "S": "2023-10-29T13:09:53.543Z"
            },
            "updatedAt": {
                "S": "2023-10-29T13:09:53.545Z"
            },
            "dicData": {
                "M": {
                    "key1": {
                        "S": "JQZB3TY7LUNE"
                    },
                    "key2": {
                        "S": "QZOUGY1PVIR6"
                    },
                    "key3": {
                        "S": "EBCWMSVZKKOM"
                    }
                }
            },
            "owner_inDate": {
                "S": "2022-07-18T07:42:46.490Z"
            },
            "listData": {
                "L": [
                    {
                        "S": "WJOLDW7Q1TXW"
                    },
                    {
                        "S": "15XFDY0PSJTF"
                    },
                    {
                        "S": "SWTBY0LTRBGB"
                    }
                ]
            },
            "intData": {
                "N": "108684"
            }
        }
    ],
    // 불러오기를 만료한 이후에 다음 불러오기를 할 시점
    "firstKey": {
        "inDate": {
            "S": "2023-10-29T13:09:53.543Z"
        }
    },
    "ConsumedCapacity": {
        "Read": {
            "CapacityUnits": 0.5,
            "GlobalSecondaryIndexes": {
                "partition-inDate-index": {
                    "CapacityUnits": 0.5
                }
            }
        }
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
    var userBro = Backend.Social.GetUserInfoByNickName("nickName");
    string gamerIndate = userBro.GetReturnValuetoJSON()["row"]["inDate"].ToString();

    var bro = Backend.PlayerData.GetOtherData("PlayerInfo", gamerIndate);

    if(!bro.IsSuccess())
    {
        Debug.LogError(bro.ToString());
        return;
    }

    LitJson.JsonData gameDataListJson = bro.FlattenRows();

    if(gameDataListJson.Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }

    GameDataItem gameDataItem = new GameDataItem(gameDataListJson[0]);
    Debug.Log(gameDataItem.ToString());
}
```
