---
sidebar_label: "데이터 조회 (쿼리이용)"
description: "GetMyData"
---

# GetMyData
# GetMyData
public BackendReturnObject **GetMyData**(string **tableName**, Where **where**);  
public BackendReturnObject **GetMyData**(string **tableName**, Where **where**, int **limit**);  
public BackendReturnObject **GetMyData**(string **tableName**, Where **where**, int **limit**, string **firstKey**);  
public BackendReturnObject **GetMyData**(string **tableName**, Where **where**, string[] **select**, int **limit**);  
public BackendReturnObject **GetMyData**(string **tableName**, Where **where**,  string[] **select**, int **limit**, string **firstKey**);  




## 파라미터

| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :--- |
| tableName   | string | 조회할 테이블명 | - |
| where | [Where](/sdk-docs/backend/base/game-information/clause-where/basic)  | 검색할 where 절 | - |
| limit | int | (Optional) 불러올 게임 정보 row 수. 최소 1, 최대 100. | 10 |
| select | string[] | (Optional) row 내 존재하는 컬럼 중 포함 시키고자 하는 컬럼 | 모든 컬럼을 포함 |
| [firstKey](/sdk-docs/backend/base/knowhow/object-for-return/paging/basic) | string | (Optional) 데이터를 조회하기 위한 시작점 | 제일 마지막에 insert 된 데이터 |


## 설명
자기 자신의 데이터를 조회합니다.  
* 스키마 정의/미정의 여부 관계없이 데이터를 조회할 수 있습니다.  
* public/private 여부 관계없이 자기 자신의 데이터만 조회할 수 있습니다.  
* select 절의 사용 유무와 관계없이 데이터의 처리량은 동일합니다.  

### 조건 없이 모든 데이터 조회
조건 없이 모든 데이터를 검색하고자 할 때는 인자 값으로 넘기는 where을 **new Where()** 로 선언해 주면 모든 데이터를 검색할 수 있습니다.  

## Example 1
조건없이 내 모든 데이터를 조회
### 동기
```js
var bro = Backend.GameData.GetMyData("tableName", new Where(), 10);
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
        string inDate = bro.FlattenRows()[0]["inDate"].ToString();
        Debug.Log(inDate);
}
```

### 비동기
```js
Backend.GameData.GetMyData("tableName", new Where(), 10, bro =>
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
        string inDate = bro.FlattenRows()[0]["inDate"].ToString();
        Debug.Log(inDate);
    }
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.GameData.GetMyData, "tableName", new Where(), 10, bro =>
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
        string inDate = bro.FlattenRows()[0]["inDate"].ToString();
        Debug.Log(inDate);
    }
});
```

## Example 2
내 데이터 중 특정 조건을 만족하는 데이터만 검색
### 동기
```js
// type 컬럼이 weapon이고,
// name 컬럼이 dragon으로 시작하는 데이터 조회
Where where = new Where();
where.Equal("type", "weapon");
where.BeginsWith("name", "dragon");

var bro = Backend.GameData.GetMyData("tableName", where, 10);
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
//데이터 추출 방법 1 : 검색한 데이터 중 첫번째 데이터의 name 컬럼 출력
string name = bro.Rows()[0]["name"]["S"].ToString();
int level = int.Parse(bro.Rows()[0]["level"]["N"].ToString());

//데이터 추출 방법 2(언마샬) : 검색한 데이터 중 첫번째 데이터의 name 컬럼 출력
string name = bro.FlattenRows()[0]["name"].ToString();
int level = int.Parse(bro.FlattenRows()[0]["level"].ToString());


Debug.Log(name);
```

### 비동기
```js
// type 컬럼이 weapon이고,
// name 컬럼이 dragon으로 시작하는 데이터 조회
Where where = new Where();
where.Equal("type", "weapon");
where.BeginsWith("name", "dragon");

Backend.GameData.GetMyData("tableName", where, 10, bro =>
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
        //데이터 추출 방법 1 : 검색한 데이터 중 첫번째 데이터의 name 컬럼 출력
        string name = bro.Rows()[0]["name"]["S"].ToString();
        int level = int.Parse(bro.Rows()[0]["level"]["N"].ToString());

        //데이터 추출 방법 2(언마샬) : 검색한 데이터 중 첫번째 데이터의 name 컬럼 출력
        string name = bro.FlattenRows()[0]["name"].ToString();
        int level = int.Parse(bro.FlattenRows()[0]["level"].ToString());
});
```

### SendQueue
```js
// type 컬럼이 weapon이고,
// name 컬럼이 dragon으로 시작하는 데이터 조회
Where where = new Where();
where.Equal("type", "weapon");
where.BeginsWith("name", "dragon");

SendQueue.Enqueue(Backend.GameData.GetMyData, "tableName", where, 10, bro =>
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
        //데이터 추출 방법 1 : 검색한 데이터 중 첫번째 데이터의 name 컬럼 출력
        string name = bro.Rows()[0]["name"]["S"].ToString();
        int level = int.Parse(bro.Rows()[0]["level"]["N"].ToString());

        //데이터 추출 방법 2(언마샬) : 검색한 데이터 중 첫번째 데이터의 name 컬럼 출력
        string name = bro.FlattenRows()[0]["name"].ToString();
        int level = int.Parse(bro.FlattenRows()[0]["level"].ToString());
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

**존재하지 않는 테이블에 GetMyData을 시도한 경우**  
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
    var bro = Backend.GameData.GetMyData("PlayerInfo", new Where());

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
