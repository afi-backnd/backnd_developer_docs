---
sidebar_label: 모든 데이터 불러오기
---

# GetAllData
public Task&lt;GetDataResult&gt; **GetAllDataAsync**(string **tableName**);  
public Task&lt;GetDataResult&gt; **GetAllDataAsync**(string **tableName**, int **limit**);  
public Task&lt;GetDataResult&gt; **GetAllDataAsync**(string **tableName**, int **limit**, string **offsetKey**);  
public Task&lt;GetDataResult&gt; **GetAllDataAsync**(string **tableName**, int **limit**, string **offsetKey**, TableSortOrder **sortOrder**);  
public Task&lt;GetDataResult&gt; **GetAllDataAsync**(string **tableName**, string[] **select**);  
public Task&lt;GetDataResult&gt; **GetAllDataAsync**(string **tableName**, string[] **select**, int **limit**);  
public Task&lt;GetDataResult&gt; **GetAllDataAsync**(string **tableName**, string[] **select**, int **limit**, string **offsetKey**);  
public Task&lt;GetDataResult&gt; **GetAllDataAsync**(string **tableName**, string[] **select**, int **limit**, string **offsetKey**, TableSortOrder **sortOrder**);  

## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| tableName   | string | 조회할 테이블명 | - |
| limit | int | (Optional) 불러올 게임 정보 row 수. 최소 1, 최대 100. | 10 |
| select | string[] | (Optional) row 내 존재하는 컬럼 중 포함 시키고자 하는 컬럼 | 모든 컬럼을 포함 |
| [offsetKey](/sdk-docs/backend/base/knowhow/object-for-return/paging/basic) | string | (Optional) 데이터를 조회하기 위한 시작점 | 제일 마지막에 insert 된 데이터 |
| sortOrder | TableSortOrder | (Optional) TableSortOrder.DESC(내림차순) or TableSortOrder.ASC(오름차순) | TableSortOrder.DESC(내림차순) |

## 설명
테이블에 저장되어 있는 값 중 limit 수만큼 모든 값을 불러옵니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 조회할 수 있습니다.  
* private 테이블의 경우 자기 자신의 데이터만 조회할 수 있습니다.  
* public 테이블의 경우, 타인의 데이터도 함께 조회됩니다.
* select 절의 사용 유무와 관계없이 데이터의 처리량은 동일합니다.  

## Example

### Task 형식
```js
 GetDataResult reqResult = null;

// tableName에서 최대 10개의 row 불러오기
reqResult = await BackndUserData.Instance.GetAllDataAsync(tableName);
// 불러오기에 실패할 경우
if (reqResult.IsSuccess() == false)
{
    Debug.Log("데이터 읽기 중에 문제가 발생했습니다 : " + reqResult.ToString());
}
// 불러오기에는 성공했으나 데이터가 존재하지 않는 경우
if (reqResult.IsSuccess() && reqResult.GetRows().Count <= 0)
{
    Debug.Log("데이터가 존재하지 않습니다");
}
// 1개 이상 데이터를 불러온 경우
if (reqResult.GetRows().Count > 0)
{
    string inDate = reqResult.GetRows()[0]["inDate"].ToString();
    int level = int.Parse(reqResult.GetRows()[0]["level"].ToString());
}
// tableName에서 최대 1개의 row 불러오기
reqResult = await BackndUserData.Instance.GetAllDataAsync(tableName, 1);

//  위에서 호출한 최대 1개의 row 불러오기 이후, 그 아래로 더 불러올 오래된 데이터가 있는지 확인
if (reqResult.HasOffsetKey() == false)
{
    Debug.Log("불러올 데이터가 존재하지 않습니다");
}

// 위에서 호출한 최대 1개의 row 불러오기 이후의 데이터가 존재할 경우, 그 이후의 데이터 불러오기
// (최신순 -> 오래된 순으로 데이터가 불러와집니다.)
reqResult = await BackndUserData.Instance.GetAllDataAsync(tableName, 1, reqResult.GetOffsetKey());

// 위에서 호출한 최대 1개의 row 불러오기 이후의 데이터가 존재할 경우, 그 이후의 데이터 내림차순으로 불러오기
// (최신순 -> 오래된 순으로 데이터가 불러와집니다.)
reqResult = await BackndUserData.Instance.GetAllDataAsync(tableName, 1, reqResult.GetOffsetKey(), TableSortOrder.DESC);
```

### Callback 형식
```js
string[] selectList = new string[] { "intData", "doubleData" };
BackndUserData.Instance.GetAllData(tableName, callback =>
{
    if (callback.IsSuccess() == false)
    {
        Debug.Log("데이터 읽기 중에 문제가 발생했습니다 : " + callback.ToString());
    }
    // 불러오기에는 성공했으나 데이터가 존재하지 않는 경우
    if (callback.IsSuccess() && callback.GetRows().Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
    }
    // 1개 이상 데이터를 불러온 경우
    if (callback.GetRows().Count > 0)
    {
        string inDate = callback.GetRows()[0]["inDate"].ToString();
        int level = int.Parse(callback.GetRows()[0]["level"].ToString());
    }
});

BackndUserData.Instance.GetAllData(tableName, 1, callback =>
{
    if (callback.IsSuccess() && callback.GetRows().Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }

    if (callback.HasOffsetKey() == false)
    {
        Debug.Log("이후 읽을 데이터가 존재하지 않습니다");
        return;
    }
    BackndUserData.Instance.GetAllData(tableName, 1, callback.GetOffsetKey(), callback2 =>
    {
        // callback2 처리
    });

    BackndUserData.Instance.GetAllData(tableName, 1, callback.GetOffsetKey(), TableSortOrder.DESC, callback2 =>
    {
        // callback2 처리
    });
});

BackndUserData.Instance.GetAllData(tableName, selectList, callback =>
{
    if (callback.IsSuccess() && callback.GetRows().Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
    }
});

BackndUserData.Instance.GetAllData(tableName, selectList, 1, callback =>
{
    if (callback.IsSuccess() && callback.GetRows().Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }
    if (callback.HasOffsetKey() == false)
    {
        Debug.Log("이후 읽을 데이터가 존재하지 않습니다");
        return;
    }

    BackndUserData.Instance.GetAllData(tableName, selectList, 1, callback.GetOffsetKey(), callback2 =>
    {
        // callback2 처리
    });
    BackndUserData.Instance.GetAllData(tableName, selectList, 1, callback.GetOffsetKey(), TableSortOrder.DESC, callback2 =>
    {
        // callback2 처리
    });
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

**존재하지 않는 테이블에 Get을 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : {테이블명} table not found, {테이블명} table을(를) 찾을 수 없습니다

**비활성화된 tableName에 불러오기를 시도한 경우**  
statusCode : 412  
errorCode :  PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  

## ReturnValue(rows 내부에 존재하는 값은 모두 데이터에 따라 다릅니다.)
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
public class PlayerDataItem
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

    public PlayerDataItem()
    {
    }

    public PlayerDataItem(JObject json)
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
public async Task GetInDate()
{
    var reqRequest = await BackndUserData.Instance.GetAllDataAsync("PlayerInfo");
    if (!reqRequest.IsSuccess())
    {
        Debug.LogError(reqRequest.ToString());
        return;
    }

    var gameDataListJson = reqRequest.GetRows();
    if (gameDataListJson.Count <= 0)
    {
        Debug.Log("데이터가 존재하지 않습니다");
        return;
    }

    var gameDataItem = new PlayerDataItem(gameDataListJson[0] as JObject);
    Debug.Log(gameDataItem.ToString());
}
```
