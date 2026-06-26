---
sidebar_label: 특정 길드 정보 조회
---

# GetGuildInfo

public Task< GetGuildInfoResult > **GetGuildInfoAsync**(string **guildIndate**);

## 파라미터

| Value       | Type   | Description                 |
| ----------- | ------ | --------------------------- |
| guildIndate | string | 정보를 받아올 길드의 indate |

## 설명

해당 guildIndate를 가진 길드의 정보를 조회합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.GetGuildInfoAsync("guildIndate");
```

### Callback 방식

```js
BackndGuild.Instance.GetGuildInfo("guildIndate", (callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

### Error cases

**guildIndate가 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다

**old guild를 조회한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : guild's version is different 사전 조건을 만족하지 않습니다.  

## ReturnValueJson

```js
{
    // 길드 정보
    guild:
    {
        // 길드원 수
        memberCount:{ N: "5" },
        // 부 길드 마스터 리스트
        viceMasterList:
        {
            L:
            [
                {
                    M:
                    {
                        // 부 길드 마스터 indate
                        inDate: { S: "2019-02-25T06:29:28.849Z" },
                        // 부 길드 마스터 닉네임
                        nickname: { S: "id23" }
                    }
                },
                {
                    M:
                    {
                        inDate: { S: "2019-02-25T06:29:27.533Z" },
                        nickname: { S: "id22" }
                    }
                }
            ]
        },
        // 길드 마스터 닉네임
        masterNickname: { S: "id24" },
        // 길드의 indate
        inDate: { S: "2019-04-12T07:38:16.522Z" },
        // 길드 명
        guildName: { S: "mumin" },
        // 길드 굿즈 수량
        goodsCount:{ N: "2" },
        // 즉시 가입 여부(SetRegistrationValueV3를 통해 설정 한 경우만 존재)
        _immediateRegistration: { BOOL: "true"},
        // 국가 코드(국가 코드를 추가한 경우에만 존재)
        _countryCode: {S: "KR"},
        // 길드 마스터의  indate
        masterInDate: { S: "2019-02-25T06:29:30.022Z" }
    }
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
public async Task GetGuildInfoV3()
{
    var reqResult = await BackndGuild.Instance.GetGuildInfoAsync("2021-12-16T08:51:00.706Z");
    if (!reqResult.IsSuccess())
        return;

    var info = reqResult.GetInfo();
    List<GuildItem> guildList = new List<GuildItem>();

    GuildItem guildItem = new GuildItem();
    guildItem.memberCount = info.MemberCount;
    guildItem.masterNickname = info.MasterNickname;
    guildItem.inDate = info.InDate;
    guildItem.guildName = info.GuildName;
    guildItem.goodsCount = info.GoodsCount;
    guildItem._immediateRegistration = info.IsImmediateRegistration;
    guildItem._countryCode = info.CountryCode;
    guildItem.masterInDate = info.MasterInDate;
    var viceInfoList = info.ViceMasterInfoList;
    for (int j = 0; j < viceInfoList.Count; j++)
    {
        guildItem.viceMasterList.Add(viceInfoList[j].InDate, viceInfoList[j].Nickname);
    }

    guildList.Add(guildItem);
    Debug.Log(guildItem.ToString());
}
```
