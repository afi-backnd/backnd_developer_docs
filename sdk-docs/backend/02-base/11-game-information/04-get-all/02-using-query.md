---
sidebar_label: 쿼리로 전체 데이터 조회
---

# Get

public BackendReturnObject **Get**(string **tableName**, Where **where**);  
public BackendReturnObject **Get**(string **tableName**, Where **where**, int **limit**);  
public BackendReturnObject **Get**(string **tableName**, Where **where**, int **limit**, string **firstKey**);  
public BackendReturnObject **Get**(string **tableName**, Where **where**, int **limit**, string **firstKey**, TableSortOrder **tableSortOrder**);  
public BackendReturnObject **Get**(string **tableName**, Where **where**, string[] **select**);  
public BackendReturnObject **Get**(string **tableName**, Where **where**, string[] **select**, int **limit**);  
public BackendReturnObject **Get**(string **tableName**, Where **where**, string[] **select**, int **limit**, string **firstKey**);  
public BackendReturnObject **Get**(string **tableName**, Where **where**, string[] **select**, int **limit**, string **firstKey**, TableSortOrder **tableSortOrder**);  

## 파라미터

| Value        | Type           | Description  | Defalut |
| :------------ |:-------------| :----- | :--- |
| tableName   | string | 조회할 테이블명 | - |
| where | [Where](/sdk-docs/backend/base/game-information/clause-where/basic)  | 검색할 where 절 | - |
| limit | int | (Optional) 불러올 게임 정보 row 수. 최소 1, 최대 100. | 10 |
| select | string[] | (Optional) row 내 존재하는 컬럼 중 포함 시키고자 하는 컬럼 | 모든 컬럼을 포함 |
| [firstKey](/sdk-docs/backend/base/knowhow/object-for-return/paging/basic) | string | (Optional) 데이터를 조회하기 위한 시작점 | 제일 마지막에 insert 된 데이터 |
| sortOrder | TableSortOrder | (Optional) TableSortOrder.DESC(내림차순) or TableSortOrder.ASC(오름차순) | TableSortOrder.DESC(내림차순) |


## 설명
테이블에 저장되어 있는 데이터를 조회합니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 조회할 수 있습니다.  
* public/private 여부 관계없이 데이터를 조회할 수 있습니다.  
* private 테이블의 경우 자기 자신의 데이터만 조회할 수 있습니다.  
* select 절의 사용 유무와 관계없이 데이터의 처리량은 동일합니다.  

### 조건 없이 모든 데이터 조회
조건 없이 모든 데이터를 검색하고자 할 때는 인자 값으로 넘기는 where을 **new Where()** 로 선언해주면 모든 데이터를 검색할 수 있습니다.  

### 특정 유저의 데이터만 조회
특정 유저의 퍼블릭 테이블만 조회하고 싶을경우 아래와 같이 where을 선언하면 됩니다.  
```js
Where where = new Where();
where.Equal("owner_inDate", "유저의 gamerInDate");
```

* owner_inDate의 경우 유저의 key값을 이용한 검색으로, 조건문에 해당 조건이 추가될 경우 검색 속도가 상승될 수 있습니다.  
* gamerIndate는 [GetUserInfoByNickName](/sdk-docs/backend/base/find-user/find-by-nickname) 함수를 통해 확인하거나, 친구, 길드원 조회 등의 함수에서 리턴되는 유저의 정보에서 확인할 수 있습니다.  

## Example 1
조건 없이 모든 데이터 검색
### 동기
```js
// 조건 없이 모든 데이터 조회하기
var bro = Backend.GameData.Get("tableName", new Where(), 10);
if(bro.IsSuccess() == false)
{
    // 요청 실패 처리
    Debug.Log(bro);
    return;
}
if(bro.GetReturnValuetoJSON()["rows"].Count <= 0)
{
    // 요청이 성공해도 where 조건에 부합하는 데이터가 없을 수 있기 때문에
    // 데이터가 존재하는지 확인
    // 위와 같은 new Where() 조건의 경우 테이블에 row가 하나도 없으면 Count가 0 이하 일 수 있다.  
    Debug.Log(bro);
    return;
}

// 검색한 데이터의 모든 row의 inDate 값 확인
for(int i=0; i<bro.Rows().Count; ++i)
{
    var inDate = bro.Rows()[i]["inDate"]["S"].ToString();
    Debug.Log(inDate);
}
```

### 비동기
```js
// 조건 없이 모든 데이터 조회하기
Backend.GameData.Get("tableName", new Where(), 10, bro =>
{
    if(bro.IsSuccess() == false)
    {
        // 요청 실패 처리
        Debug.Log(bro);
        return;
    }
    if(bro.GetReturnValuetoJSON()["rows"].Count <= 0)
    {
        // 요청이 성공해도 where 조건에 부합하는 데이터가 없을 수 있기 때문에
        // 데이터가 존재하는지 확인
        // 위와 같은 new Where() 조건의 경우 테이블에 row가 하나도 없으면 Count가 0 이하 일 수 있다.  
        Debug.Log(bro);
        return;
    }
    // 검색한 데이터의 모든 row의 inDate 값 확인
    for(int i = 0; i < bro.Rows().Count; ++i)
    {
        var inDate = bro.Rows()[i]["inDate"]["S"].ToString();
        Debug.Log(inDate);
    }
});
```

### SendQueue
```js
// 조건 없이 모든 데이터 조회하기
SendQueue.Enqueue(Backend.GameData.Get, "tableName", new Where(), 10, bro =>
{
    if(bro.IsSuccess() == false)
    {
        // 요청 실패 처리
        Debug.Log(bro);
        return;
    }
    if(bro.GetReturnValuetoJSON()["rows"].Count <= 0)
    {
        // 요청이 성공해도 where 조건에 부합하는 데이터가 없을 수 있기 때문에
        // 데이터가 존재하는지 확인
        // 위와 같은 new Where() 조건의 경우 테이블에 row가 하나도 없으면 Count가 0 이하 일 수 있다.  
        Debug.Log(bro);
        return;
    }
    // 검색한 데이터의 모든 row의 inDate 값 확인
    for(int i = 0; i < bro.Rows().Count; ++i)
    {
        var inDate = bro.Rows()[i]["inDate"]["S"].ToString();
        Debug.Log(inDate);
    }
});
```

## Example 2
특정 유저의 데이터 중 deck과 score 컬럼 만 조회
### 동기
```js
// select[]를 이용하여 리턴 시, deck과 score만 출력되도록 설정
string[] select = { "deck", "score" };
// 특정 유저의 데이터 조회를 위한 where 절
Where where = new Where();
where.Equal("owner_inDate", "user의 inDate 값(user key값)");

// 특정 유저의 데이터 중 deck과 score 컬럼만 조회
var bro = Backend.GameData.Get("tableName", where, select, 10);
if(bro.IsSuccess() == false)
{
    // 요청 실패 처리
    Debug.Log(bro);
    return;
}
if(bro.GetReturnValuetoJSON()["rows"].Count <= 0)
{
    // 요청이 성공해도 where 조건에 부합하는 데이터가 없을 수 있기 때문에
    // 데이터가 존재하는지 확인
    Debug.Log(bro);
    return;
}
// 검색한 조건 중 1번째 row의 score 확인
int score = (int)bro.Rows()[0]["score"]["N"];
```

### 비동기
```js
// select[]를 이용하여 리턴 시, deck과 score만 출력되도록 설정
string[] select = { "deck", "score" };
// 특정 유저의 데이터 조회를 위한 where 절
Where where = new Where();
where.Equal("owner_inDate", "user의 inDate 값(user key값)");

// 특정 유저의 데이터 중 deck과 score 컬럼만 조회
Backend.GameData.Get("tableName", where, select, 10, bro =>
{
    if(bro.IsSuccess() == false)
    {
        // 요청 실패 처리
        Debug.Log(bro);
        return;
    }    if(bro.GetReturnValuetoJSON()["rows"].Count <= 0)
    {
        // 요청이 성공해도 where 조건에 부합하는 데이터가 없을 수 있기 때문에
        // 데이터가 존재하는지 확인
        Debug.Log(bro);
        return;
    }    // 검색한 조건 중 1번째 row의 score 확인
    int score = (int)bro.Rows()[0]["score"]["N"];
});
```

### SendQueue
```js
// select[]를 이용하여 리턴 시, deck과 score만 출력되도록 설정
string[] select = { "deck", "score" };
// 특정 유저의 데이터 조회를 위한 where 절
Where where = new Where();
where.Equal("owner_inDate", "user의 inDate 값(user key값)");

// 특정 유저의 데이터 중 deck과 score 컬럼만 조회
SendQueue.Enqueue(Backend.GameData.Get, "tableName", where, select, 10, bro =>
{
    if(bro.IsSuccess() == false)
    {
        // 요청 실패 처리
        Debug.Log(bro);
        return;
    }
    if(bro.GetReturnValuetoJSON()["rows"].Count <= 0)
    {
        // 요청이 성공해도 where 조건에 부합하는 데이터가 없을 수 있기 때문에
        // 데이터가 존재하는지 확인
        Debug.Log(bro);
        return;
    }
    // 검색한 조건 중 1번째 row의 score 확인
    int score = (int)bro.Rows()[0]["score"]["N"];
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**조회에 성공했으나 해당 where 검색 조건에 해당하는 데이터가 존재하지 않는 경우**  
statusCode : 200  
message : Success  
returnValue : {"serverTime":"2021-03-23T07:07:01.235Z","rows":[],"firstKey":null}

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
    "rows":[
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
            ... // 클라이언트에 저장된 데이터
        },
        // and etc...  
    ],

    // firstKey가 존재하지 않는 경우 null
    "firstKey":null

    // firstKey가 존재하는 경우
    // "firstKey" : {"inDate":{"S":"2021-03-23T04:54:45.849Z"}}}
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
public void GetWhereTest()
{
    var bro = Backend.GameData.Get("PlayerInfo", new Where());

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
