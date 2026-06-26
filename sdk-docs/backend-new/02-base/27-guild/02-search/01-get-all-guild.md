---
sidebar_label: 모든 길드 리스트 조회
description: "모든 길드 리스트 조회"
---

# GetGuilds

public Task< GetGuildsResult > **GetGuildsAsync**();  
public Task< GetGuildsResult > **GetGuildsAsync**(int **limit**);  
public Task< GetGuildsResult > **GetGuildsAsync**(string **firstKey**);  
public Task< GetGuildsResult > **GetGuildsAsync**(int **limit**, string **firstKey**);

## 파라미터

| Value    | Type   | Description                                    | default |
| -------- | ------ | ---------------------------------------------- | :-----: |
| limit    | int    | (Optional) 불러올 길드 리스트의 수             |   100   |
| firstKey | string | (Optional) 불러올 길드 리스트의 시작점(offset) |    -    |

## 설명

모든 길드 리스트를 조회합니다.  
길드 리스트에는 각 길드의 메타 데이터가 포함되어 리턴됩니다.  
* limit이 0 이하일 경우 100으로 호출됩니다.

## Example

### Task 방식

```js
var reqResult = await BackndGuild.Instance.GetGuildsAsync();
reqResult = await BackndGuild.Instance.GetGuildsAsync(5);

// 처음 리스트를 읽을 때, firstkey 값을 저장해 놓은 뒤,
reqResult = await BackndGuild.Instance.GetGuildsAsync();
var firstKey = reqResult.GetOffsetKey();
// 다음 페이지 넘어갈 때 firstKey를 offset으로 하여 다시 읽어옴
reqResult = await BackndGuild.Instance.GetGuildsAsync(firstKey);
reqResult = await BackndGuild.Instance.GetGuildsAsync(5, firstKey);
```

### Callback 방식

```js
BackndGuild.Instance.GetGuilds((callback) =>
{
  // 이후 처리
});
BackndGuild.Instance.GetGuilds(5, (callback) =>
{
  // 이후 처리
});

// 동기 함수와 같은 방식으로 firstKey를 저장한 후, 다음 리스트를 불러옴
BackndGuild.Instance.GetGuilds(firstKey, (callback) =>
{
  // 이후 처리
});
BackndGuild.Instance.GetGuilds(20, firstKey, (callback) =>
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

## ReturnValueJson

```js
{
    rows:
    [
        // new guild(v2, v3 버전으로 만들어진 길드)
        {
            // 길드원 수
            memberCount:{ N: "5" },
            // 부 길드 마스터 리스트
            viceMasterList: {
                L:[
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
                            nickname: { S: "id22"}
                        }
                    }
                ]
            },
            // 길드 마스터 닉네임
            masterNickname: { S: "id24" },
            // 길드의 inDate
            inDate: { S: "2019-04-12T07:38:16.522Z" },
            // 길드명
            guildName: { S: "mumin" },
            // 길드 굿즈 수량
            goodsCount: { N: "2"},
            // 길드 메타 데이터
            buf: { N: "1" },
            // 길드 메타 데이터
            level: { S: "silver" },
            // 즉시 가입 여부(SetRegistrationValueV3를 통해 설정 한 경우만 존재)
            _immediateRegistration: { BOOL: "true"},
            // 국가 코드(국가 코드를 추가한 경우에만 존재)
            _countryCode: {S: "KR"}
            // 길드 마스터의 inDate
            masterInDate: { S: "2019-02-25T06:29:30.022Z" }
        },
        // old guild
        {
            // 길드명
            guildName: { S: "guildName" },
            // 길드의 굿즈 1 총량(기부+사용)
            totalGoods1Amount: { N: "0" },
            // 길드의 굿즈 2 총량(기부+사용)
            totalGoods2Amount: { N: "0" },
            // 길드의 굿즈 3 총량(기부+사용)
            totalGoods3Amount: { N: "0" },
            // 길드원 수
            memberCount: { N: "2" },
            // 부 길드 마스터 리스트
            viceMasterList: {
                L:[
                    {
                        M:{
                            // 부 길드 마스터 indate
                            inDate: { S: "2018-07-31T01:19:49.193Z" },
                            // 부 길드 마스터 닉네임
                            nickname: { S: "customid1" }
                        }
                    }
                ]
            },
            // 길드 메타 데이터
            buf: { N: "1" },
            // 길드 마스터 닉네임
            masterNickname: { S: "masterNickname" },
            // 길드의 inDate
            inDate: { S: "2018-07-13T07:04:39.650Z" },
            // 길드 메타 데이터
            level: { S: "gold" },
            // 길드 마스터의 inDate
            masterInDate: { S: "2018-06-25T03:19:01.706Z" }
        }
    ],
    firstKey: { // 리스트를 페이징 처리하기 위한 offset key(리스트의 끝까지 불러온 경우 리턴되지 않음)
        inDate: { S: "2018-07-13T07:04:39.650Z" }
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
public async Task GetGuildListV3()
{
    var reqResult = await BackndGuild.Instance.GetGuildsAsync();
    if (!reqResult.IsSuccess())
        return;

    var infoList = reqResult.GetInfoList();
    List<GuildItem> guildList = new List<GuildItem>();

    for (int i = 0; i < infoList.Count; i++)
    {
        var info = infoList[i];

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
}
```
