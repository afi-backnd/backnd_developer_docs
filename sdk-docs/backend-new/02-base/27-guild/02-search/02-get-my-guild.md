---
sidebar_label: 내 길드 정보 조회
description: "내 길드 정보 조회"
---

# GetUserGuildInfo

public BackendReturnObject **GetUserGuildInfoAsync**();

## 설명

현재 게이머가 속한 길드의 정보를 조회합니다.  
길드에 속해 있지 않은 경우 **statusCode 412, errorCode PreconditionFailed**을 반환합니다.  

> 길드 굿즈 정보는 반환하지 않습니다.  

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.GetUserGuildInfoAsync();
```

### Callback 방식

```js
BackndGuild.Instance.GetUserGuildInfo((callback) =>
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

**Old guild의 유저가 조회한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : guild's version is different 사전 조건을 만족하지 않습니다.  

**guild가 없는 유저가 조회한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : notGuildMember 사전 조건을 만족하지 않습니다.  

## ReturnValueJson

```js
{
    // 길드 정보
    guild:
    {
        // 길드원 수
        memberCount:{ N: "5" },
        // 길드 메타 데이터
        buf: { N: "1" },
        // 길드 메타 데이터
        level: { S: "silver" },
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
        // 길드 마스터의  indate
        masterInDate: { S: "2019-02-25T06:29:30.022Z" }
        // 즉시 가입 여부(SetRegistrationValueV3를 통해 설정 한 경우만 존재)
        _immediateRegistration: { BOOL: "true"},
        // 국가 코드(국가 코드를 추가한 경우에만 존재)
        _countryCode: {S: "KR"}
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
public async Task GetMyGuildInfoV3()
{
    var reqResult = await BackndGuild.Instance.GetUserGuildInfoAsync();
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
