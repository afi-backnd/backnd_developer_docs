---
sidebar_label: "[Deprecated] 길드 랜덤 조회"
draft: true
unlisted: true
---

# GetRandomGuildInfoV3

public BackendReturnObject **GetRandomGuildInfoV3**(int **limit**);

## 파라미터

| Value | Type | Description                          |
| ----- | ---- | ------------------------------------ |
| limit | int  | 불러올 랜덤 길드 정보의 수. 최대 100 |

## 설명

생성된 길드들을 랜덤으로 limit 만큼 조회합니다.  
limit는 최대 100이며 그 이상으로 설정하실 경우 자동으로 100으로 고정됩니다.  
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
var bro = Backend.Guild.GetRandomGuildInfoV3(20);

//example
for(int i=0; i<bro.Rows().Count; i++)
{
  string guildIndate = bro.Rows()[i]["inDate"].ToString();
  string guildName = bro.Rows()[i]["guildName"].ToString();
  string metaData = bro.Rows()[i]["메타 정보"].ToString();
}
```

### 비동기

```js
Backend.Guild.GetRandomGuildInfoV3(20, (callback) =>
{
    // 이후 처리

    //example
    for(int i=0; i<callback.Rows().Count; i++)
    {
      string guildIndate = callback.Rows()[i]["inDate"].ToString();
      string guildName = callback.Rows()[i]["guildName"].ToString();
      string metaData = callback.Rows()[i]["메타 정보"].ToString();
    }
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.GetRandomGuildInfoV3,20, (callback) =>
{
    // 이후 처리

    //example
    for(int i=0; i<callback.Rows().Count; i++)
    {
            string nickname = callback.Rows()[i]["nickname"].ToString();
            string guildName = callback.Rows()[i]["guildName"].ToString();
    }
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

** metaKey, Value의 조건에 맞는 길드가 없을 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

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
            "guildPower":100, //설정한 메타 정보
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
    var bro = Backend.Guild.GetRandomGuildInfoV3(100);

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
