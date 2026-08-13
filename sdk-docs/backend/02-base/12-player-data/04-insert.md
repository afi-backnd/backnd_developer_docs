---
sidebar_label: "유저 데이터 등록"
description: "InsertData"
---

# InsertData

public BackendReturnObject **InsertData**(string **tableName**);  
public BackendReturnObject **InsertData**(string **tableName**, Param **param**);

## 파라미터

| Value     | Type                                            | Description                 |
| :-------- | :---------------------------------------------- | :-------------------------- |
| tableName | string                                          | 게임 정보를 저장할 테이블명 |
| param     | [Param](/sdk-docs/backend/base/knowhow/param/Param) | 게임 정보에 저장할 내용     |

## 설명

테이블에 row을 추가합니다.  

- 해당 row는 InsertData 한 유저에게 귀속됩니다.  
- 스키마 정의/미정의 여부 관계없이 데이터를 등록할 수 있습니다.  
- public/private 여부 관계없이 데이터를 등록할 수 있습니다.  
- 스키마 등록 시, 값이 비어있을 경우 null 또는 기본값으로 입력됩니다.  
- 숫자형 데이터의 경우, 최대 9007199254740991(약 9000조)까지 안전하게 값을 넣을 수 있습니다.  
  그 이상의 데이터를 등록할 경우, 일의 자리, 십의 자리 수는 0으로 내림처리가 됩니다.  
  숫자가 증가하여 자리 수가 많아질수록 작은 수는 내림처리가 되며 큰수는 왼쪽 기준으로 16~18자리의 수까지 값이 유지됩니다.  
  > long의 경우 다음과 같이 데이터가 저장됩니다.  

  > 12345678912345678 -> 12345678912345678(9000조 이하, 기본값 유지)
  > 123456789123456789 -> 123456789123456780(9000조 이상, 내림 처리 발생)
  > 1234567891234567891 -> 1234567891234568000
  > 9223372036854775807 -> 9223372036854776000(long의 최대값)  

  > 더 큰 데이터 저장을 위해 double로 정수를 저장할 경우 다음과 같이 표시됩니다.  

  > 12345678912345678912 -> 12345678912345678000(long의 최대값 보다 더 큰 double)
  > 123456789123456789123.0 -> 123456789123456800000
  > 1234567891234567891234.0 -> 123456789123456800000000
  > 1.23456789123457E+29 -> 123456789123457000000000000000  

  > 해당 수를 클라이언트(C#)에서 받을 경우 지수형태로 변경이 될 수 있습니다.  
  > 12345678912345678000 -> 1.23456789123457E+19

### 예약된 컬럼

저장 시, **partition, gamer_id, inDate, updatedAt, sender, receiver, reservationDate, owner_inDate, client_date** 필드는 서버에서 사용하는 컬럼입니다.  
해당 컬럼이 param에 포함된 경우 param에 있는 값은 무시하고, 서버에서 사용하는 값이 저장됩니다.  

| Value | Description | 뒤끝 콘솔 노출 여부 |  
| :----- | :----- | :----- |  
| partition | 서버에서 테이블을 구분하기 위해 사용하는 값 | X |  
| gamer_id | row 소유자의 게이머 ID | O |  
| inDate | row의 inDate (key 값) | O |  
| updatedAt | 테이블이 마지막으로 수정된 시각 | O |  
| sender | 뒤끝 기능을 위해 내부에서 사용하는 값 (우편 등) | X |  
| receiver | 뒤끝 기능을 위해 내부에서 사용하는 값 (우편 등) | X |  
| reservationDate | 뒤끝 기능을 위해 내부에서 사용하는 값 (우편 등) | X |  
| owner_inDate | row 소유자의 유저 inDate | O |  
| client_date | 처음으로 클라이언트에서 데이터 생성을 요청한 시간 | O |  


### 스키마 테이블의 경우
스키마 테이블을 사용할 경우 다음과 같은 주의사항이 있습니다.
* 데이터 형식에 맞지 않는 데이터를 등록할 경우, 에러가 발생합니다.
* 데이터 형식을 string으로 설정할 경우, 데이터 10은 등록할 수 없습니다.
* 데이터 형식을 int, float, double로 설정할 경우, 데이터 "10"은 등록할 수 없습니다.
* 데이터 형식을 int로 설정한 경우, 데이터 3.14를 등록한다면 데이터 3으로 수정됩니다. (소수점 자리가 버림처리 됩니다.)
* 데이터 형식을 DateTime으로 설정할 경우, 해당 형식과 동일한 데이터 형식만 등록 가능합니다. (HH:mm:ss로 설정한 경우, "15:21:31" 데이터만 등록 가능하며, "2024-01-23T15:21:31.311Z" 혹은 "2024-01-23 15:21:31"와 같은 데이터는 등록 불가합니다.
* 모든 데이터 형식은 param.Add("item", null) 과 같이 null로 등록/수정이 불가능합니다. (bad {data} dataType, 잘못된 {data} dataType 입니다 에러가 발생합니다.)
* null 허용일 경우, 데이터 삽입 시 param에 해당 데이터가 존재하지 않으면 해당 컬럼이 존재하지 않은 채로 데이터가 생성됩니다.
* null 비허용일 경우, 데이터 삽입 시 param에 해당 데이터가 존재하지 않으면 자동으로 기본값이 삽입됩니다. 다만 list 혹은 map의 경우 데이터 삽입 시, 무조건 해당 데이터가 포함되어 있어야 합니다.
* DateTime의 경우, null이 비허용이면서 기본값이 ON일 경우 현재 시간이 UTC 기준으로 설정됩니다. (한국 시간 11:35일 경우, 02:35으로 설정됩니다.) 기본값이 OFF일 경우, 해당 컬럼을 포함하지 않고 데이터 삽입을 시도할 경우 에러가 발생합니다.

## Example

### param

```js
Param lunch = new Param();
lunch.Add("how much", 332);
lunch.Add("when", "yesterday");
lunch.Add("what", "eat chocolate");

Dictionary<string, int> dic = new Dictionary<string, int>
{
    { "dic1", 1 },
    { "dic4", 2 },
    { "dic2", 4 }
};

Dictionary<string, string> dic2 = new Dictionary<string, string>
{
    { "mm", "j" },
    { "nn", "n" },
    { "dd", "2" }
};

String[] list = { "a", "b" };
int[] list2 = { 400, 500, 600 };

Param param = new Param();
param.Add("이름", "cheolsu");
param.Add("score", 99);
param.Add("lunch", lunch);
param.Add("dic_num", dic);
param.Add("dic_string", dic2);
param.Add("list_string", list);
param.Add("list_num", list2);
```

### 동기

```js
Backend.PlayerData.InsertData("tableName", param);
```

### 비동기

```js
Backend.PlayerData.InsertData("tableName", param, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.PlayerData.InsertData, "tableName", param, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**테이블 등록의 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : {"inDate":"등록한 테이블의 inDate"}

> 예) returnValue : {"ConsumedCapacity":{"Write":{"CapacityUnits":3}},"inDate":"2023-10-28T11:00:38.611Z"}

### Error cases

**(스키마) 스키마를 정의할 때 선언한 컬럼의 데이터 타입과 InsertData 하려는 데이터 타입이 다른 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad {column 명} dataType, 잘못된 {column 명} dataType 입니다

**(스키마) 스키마를 정의하지 않은 컬럼을 InsertData 하려고 시도한 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad 컬럼이 존재하지 않습니다., 잘못된 컬럼이 존재하지 않습니다.  

**(스키마) 스키마에 list 컬럼을 선언할 때 설정한 list의 크기와 param에 입력한 list의 크기가 더 클 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad list data length, 잘못된 list data length 입니다

**(스키마) 스키마에 map 컬럼을 선언할 때 선택한 map의 크기와 param에 입력한 map의 크기가 다를 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad map data length, 잘못된 map data length 입니다

**등록하려는 컬럼의 갯수가 총 290개를 넘을 경우**  
statusCode : 400  
errorCode : ValidationException  
message : Invalid UpdateExpression: Expression size has exceeded the maximum allowed size;

**등록 시도한 데이터의 크기가 400KB를 넘는 경우**  
statusCode : 400
errorCode : ValidationException
message : Item size has exceeded the maximum allowed size

**등록 시도한 데이터의 크기가 400KB를 넘는 경우**  
statusCode : 400
errorCode : ValidationException
message : Item size to update has exceeded the maximum allowed size

**존재하지 않는 테이블에 InsertData 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : {테이블명} table not found, {테이블명} table을(를) 찾을 수 없습니다

**비활성화된 tableName에 등록을 시도한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : inactiveTable 사전 조건을 만족하지 않습니다.  

**등록 시도한 데이터의 크기가 400KB를 넘는 경우**  
statusCode : 413  
errorCode : ServerErrorException  
message : request entity too large

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

    public PlayerDataItem(LitJson.JsonData json)
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
public void InsertDataTest()
{
    PlayerDataItem PlayerData = new PlayerDataItem();

    PlayerData.hp = 1000;
    PlayerData.mp = 0;
    PlayerData.atk = 231.23f;
    PlayerData.money = 100000000000;
    PlayerData.equip = new Dictionary<string, string>() { { "head", "itemID231" }, { "arms", "itemID192" }, { "legs", "itemID001" }, { "body", "itemID337" } };
    PlayerData.items = new List<string>() { "itemID231", "itemID341", "itemID12", "itemID124", "itemID331", "itemID228", "itemID775", "itemID479" };

    Param param = PlayerData.ToParam();

    var bro = Backend.PlayerData.InsertData("PlayerInfo", param);

    if(bro.IsSuccess())
    {
        string playerInfoIndate = bro.GetInDate();
        Debug.Log("내 playerInfo의 indate : " + playerInfoIndate);
    }
    else
    {
        Debug.LogError("게임 정보 등록 실패 : " + bro.ToString());
    }
}
```
