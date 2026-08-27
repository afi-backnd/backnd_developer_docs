---
sidebar_label: "[Deprecated] 메타 정보로 길드 랜덤 조회"
draft: "true"
unlisted: "true"
description: "GetRandomGuildInfoV3"
---

# GetRandomGuildInfoV3

public BackendReturnObject **GetRandomGuildInfoV3**(string **metaKey**, int **value**, int **gap**, int **limit**);  
public BackendReturnObject **GetRandomGuildInfoV3**(string **metaKey**, long **value**, long **gap**, int **limit**);

## 파라미터

| Value   | Type     | Description                                  |
| ------- | -------- | -------------------------------------------- |
| metaKey | string   | 조회할 metaKey(숫자형 데이터만 가진 metaKey) |
| value   | int/long | 기준이 되는 int값                            |
| gap     | int/long | value를 기준으로 위아래로 검색에 포함되는 값 |
| limit   | int      | 불러올 랜덤 길드 정보. 최대 100              |

## 설명

해당 metaKey에 데이터를 가지고 있는 길드들을 대상으로 value의 조건에 맞는 길드에 한해 랜덤으로 limit 만큼 조회합니다.  
value는 기준이 되는 값이며 해당 table의 column중 value와 일치하는 데이터를 가진 길드들에 한해서 랜덤으로 조회됩니다.  
gap을 설정할 경우, value를 기준으로 gap을 증감하여 최소값과 최대값을 설정할 수 있습니다.  

> \- metaValue : guildLevel,
> \- value : 100,
> \- gap: 10
> 위와 같은 설정일 경우, guildLevel이란 이름의 metaKey에 90 ~ 110 의 숫자형 데이터를 가진 길드들만을 대상으로 랜덤으로 조회합니다.  

조회가능한 길드 정보는 다음과 같습니다.  

- 길드 이름
- 굿즈 갯수
- 설정한 메타 정보
- 길드 멤버 수
- 길드 운영진 정보
- 길드 마스터의 닉네임
- 해당 길드의 inDate
- 길드 마스터의 gamerIndate
  > 자신의 길드는 검색에서 제외됩니다

## Example

### 동기

```js
//short example
var bro = Backend.Guild.GetRandomGuildInfoV3("metaKey", 10, 0, 1);

//long example
// 길드레벨이 -10 or +10인 길드 랜덤으로 20개 조회하기
string metaKey = "guildLevel";

//내 길드 정보중 guild level 데이터 가져오기
var bro = Backend.Guild.GetMyGuildInfoV4();
int myGuildLevel = int.Parse(bro.GetReturnValuetoJSON()["result"][metaKey].ToString());

// myGuildLevel이 100일 경우, 90 ~ 110 사이의 level 데이터를 가진 길드 20개 조회
var bro2 = Backend.Guild.GetRandomGuildInfoV3(metaKey, myGuildLevel, 10, 20);

for(int i=0; i<bro2.Rows().Count; i++)
{
  string guildName = bro2.Rows()[i]["guildName"].ToString();
  string guildLevel = bro2.Rows()[i][metaKey].ToString();
}
```

### 비동기

```js
//short example
Backend.Guild.GetRandomGuildInfoV3("metaKey", 10, 0, 1, callback =>
{
    // to do
});

//long example
// 길드레벨이 -10 or +10인 길드 랜덤으로 20개 조회하기
string metaKey = "guildLevel";

//내 길드 정보중 guild level 데이터 가져오기
Backend.Guild.GetMyGuildInfoV4(callback =>
{
    int myGuildLevel = int.Parse(callback.GetReturnValuetoJSON()["result"][metaKey].ToString());

    // myGuildLevel이 100일 경우, 90 ~ 110 사이의 level 데이터를 가진 길드 20개 조회
    Backend.Guild.GetRandomGuildInfoV3(metaKey, myGuildLevel, 10, 20, callback2 =>
    {
        for(int i=0; i<callback2.Rows().Count; i++)
        {
            string guildName = callback2.Rows()[i]["guildName"].ToString();
            string guildLevel = callback2.Rows()[i][metaKey].ToString();
        }
    });
});
```

### SendQueue

```js
//short example
SendQueue.Enqueue(Backend.Guild.GetRandomGuildInfoV3, "metaKey", 10, 0, 1, callback =>
{
    // to do
});

//long example
// 길드레벨이 -10 or +10인 길드 랜덤으로 20개 조회하기
string metaKey = "guildLevel";

//내 길드 정보중 guild level 데이터 가져오기
SendQueue.Enqueue(Backend.Guild.GetMyGuildInfoV4, callback =>
{
    int myGuildLevel = int.Parse(callback.GetReturnValuetoJSON()["result"][metaKey].ToString());

    // myGuildLevel이 100일 경우, 90 ~ 110 사이의 level 데이터를 가진 길드 20개 조회
    SendQueue.Enqueue(Backend.Guild.GetRandomGuildInfoV3, metaKey, myGuildLevel, 10, 20, callback2 =>
    {
        for(int i=0; i<callback2.Rows().Count; i++)
        {
            string guildName = callback2.Rows()[i]["guildName"].ToString();
            string guildLevel = callback2.Rows()[i][metaKey].ToString();
        }
    });
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**존재하지 않는 columnName일 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

**해당 조건에 만족하는 데이터가 없을 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

### Error cases

**존재하지 않는 tableName일 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : table not found, table을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

```js
{
    "rows":
    [
        {
            "guildName":"StrongGuild", // 길드 이름
            "goodsCount":3, // 굿즈 갯수
            "guildLevel":100, //설정한 메타 정보
            "comment":"가장쎈 길드", //설정한 메타 정보2
            "memberCount":1, // 길드 멤버 수
            "viceMasterList":[ // 길드 운영진
                {"nickname":"tester11","inDate":"2021-06-23T08:38:59.106Z"},
                {"nickname":"tester12","inDate":"2021-06-22T04:38:59.216Z"}
            ],
            "masterNickname":"테스터1" ,// 길드 마스터의 닉네임
            "inDate":"2020-09-01T05:54:55.752Z", // 해당 길드의 inDate
            "updatedAt":"2020-11-11T05:23:53.936Z"//길드 정보가 변경된 최신날짜
            "masterInDate":"2020-09-01T06:03:06.175Z" //길드 마스터의 gamerIndate
        },
        {
            "guildName":"GuildName", // 길드 이름
            "goodsCount":3, // 굿즈 갯수
            "guildLevel":100, //설정한 메타 정보
            "comment":"길드 메타 정보", //설정한 메타 정보2
            "memberCount":1, // 길드 멤버 수
            "viceMasterList":[], // 길드 운영진
            "masterNickname":"테스터2" // 길드 마스터의 닉네임
            "inDate":"2020-02-01T05:34:55.752Z" // 해당 길드의 inDate
            "updatedAt":"2020-11-11T05:23:53.936Z"//길드 정보가 변경된 최신날짜
            "masterInDate":"2021-01-11T06:02:06.275Z" //길드 마스터의 gamerIndate
        },
        ....  
    ]
}
```

## Sample Code

```js
public class GuildItem
{
    public int memberCount;
    public Dictionary<string, string> viceMasterList = new Dictionary<string, string>();
    public string masterNickname;
    public string inDate;
    public string guildName;
    public int goodsCount;
    public bool _immediateRegistration;
    public string _countryCode;
    public string masterInDate;
    public override string ToString()
    {
        string viceMasterString = string.Empty;
        foreach(var li in viceMasterList)
        {
            viceMasterString += $"부 길드 마스터 : {li.Value}({li.Key})\n";
        }
        return $"memberCount : {memberCount}\n" +
        $"masterNickname : {masterNickname}\n" +
        $"inDate : {inDate}\n" +
        $"guildName : {guildName}\n" +
        $"goodsCount : {goodsCount}\n" +
        $"_immediateRegistration : {_immediateRegistration}\n" +
        $"_countryCode : {_countryCode}\n" +
        $"masterInDate : {masterInDate}\n" +
        $"memberCount : {memberCount}\n" +
        viceMasterString;
    }
};
```

```js
public void GetRandomGuildInfoV3()
{
    var bro = Backend.Guild.GetRandomGuildInfoV3("metaKey", 10, 0, 1);

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData json = bro.FlattenRows();

    List<GuildItem> guildList = new List<GuildItem>();

    for(int i = 0; i < json.Count; i++)
    {
        GuildItem guildItem = new GuildItem();

        guildItem.memberCount = int.Parse(json[i]["memberCount"].ToString());
        guildItem.masterNickname = json[i]["masterNickname"].ToString();
        guildItem.inDate = json[i]["inDate"].ToString();
        guildItem.guildName = json[i]["guildName"].ToString();
        guildItem.goodsCount = int.Parse(json[i]["goodsCount"].ToString());
        if(json[i].ContainsKey("_immediateRegistration"))
        {
            guildItem._immediateRegistration = json[i]["_immediateRegistration"].ToString() == "true" ? true : false;
        }
        if(json[i].ContainsKey("_countryCode"))
        {
            guildItem._countryCode = json[i]["_countryCode"].ToString();
        }
        guildItem.masterInDate = json[i]["masterInDate"].ToString();

        LitJson.JsonData viceListJson = json[i]["viceMasterList"];
        for(int j = 0; j < viceListJson.Count; j++)
        {
            guildItem.viceMasterList.Add(viceListJson[j]["inDate"].ToString(), viceListJson[j]["nickname"].ToString());
        }

        guildList.Add(guildItem);
        Debug.Log(guildItem.ToString());
    }
}
```
