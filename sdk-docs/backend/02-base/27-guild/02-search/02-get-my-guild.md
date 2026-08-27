---
sidebar_label: "내 길드 정보 조회"
description: "GetMyGuildInfoV4"
---

# GetMyGuildInfoV4

public BackendReturnObject **GetMyGuildInfoV4**();

## 설명

현재 게이머가 속한 길드의 정보를 조회합니다.  
길드에 속해 있지 않은 경우 **statusCode 412, errorCode PreconditionFailed**을 반환합니다.  

> v2 버전의 길드와 달리, 길드가 사용하는 굿즈 종류 수(`goodsCount`)만 반환하며 굿즈 보유량은 포함하지 않습니다. 보유량은 `GetMyGuildGoodsV3`로 조회합니다.

:::caution GetMyGuildInfoV3에서 변경된 점
* 반환값의 최상위 키가 `guild`에서 `result`로 변경되었습니다. 반환값을 직접 파싱하는 경우 키 이름을 함께 수정해야 합니다.
* 반환값이 일반 JSON으로 정규화되어 내려옵니다. 따라서 `GetFlattenJSON()`이 아닌 `GetReturnValuetoJSON()`으로 바로 읽을 수 있습니다.
* 뒤끝 콘솔에 길드 마스터 자동 교체 기준 일수가 설정된 경우, `masterLastLogin`과 `inactivedMaster` 키가 추가됩니다. 자세한 내용은 [길드 마스터 자동 교체 신청](../05-guild-master/10-claim-master.md)을 참고해 주세요.
* `GetMyGuildInfoV3`는 deprecated 되었습니다. 신규 개발은 `GetMyGuildInfoV4`를 사용해 주세요.
:::

## Example

### 동기

```js
Backend.Guild.GetMyGuildInfoV4();
```

### 비동기

```js
Backend.Guild.GetMyGuildInfoV4((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.GetMyGuildInfoV4, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**길드에 가입하지 않은 유저가 조회한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : subscribed guild 사전 조건을 만족하지 않습니다.  

## GetReturnValuetoJSON

```js
{
    // 길드 정보
    result:
    {
        // 길드의 inDate
        inDate: "2026-08-21T03:18:08.682Z",
        // 길드 명
        guildName: "mumin",
        // 길드원 수
        memberCount: 5,
        // 길드 굿즈 종류 수
        goodsCount: 2,
        // 오픈 길드 여부
        isOpenGuild: false,
        // 길드 마스터의 inDate
        masterInDate: "2019-02-25T06:29:30.022Z",
        // 길드 마스터 닉네임(닉네임을 생성하지 않은 마스터는 null)
        masterNickname: "id24",
        // 부 길드 마스터 리스트
        viceMasterList:
        [
            {
                // 부 길드 마스터 inDate
                inDate: "2019-02-25T06:29:28.849Z",
                // 부 길드 마스터 닉네임
                nickname: "id23"
            }
        ],
        // 길드 메타 데이터
        level: "silver",
        // 길드 메타 데이터
        buf: 1,
        // 즉시 가입 여부(SetRegistrationValueV3로 설정한 경우에만 존재)
        _immediateRegistration: true,
        // 국가 코드(등록하지 않은 경우 "null" 문자열이 내려옵니다)
        _countryCode: "KR",
        // 길드 마스터의 마지막 로그인 시각(길드 마스터 자동 교체 기준 일수가 설정된 경우에만 존재, 로그인 기록이 없으면 null)
        masterLastLogin: "2026-08-10T12:30:00.000Z",
        // 길드 마스터의 비활성 여부(길드 마스터 자동 교체 기준 일수가 설정된 경우에만 존재)
        inactivedMaster: false,
        // 이하 서버 내부 값(게임 로직에서 사용하지 마세요)
        lowerGuildName: "mumin",
        partition: "guild",
        gamer_id: "0",
        guildMemberRequestList: []
    }
}
```

:::caution 내부 값 주의
V4 응답은 서버 레코드를 정규화해 그대로 내려주므로 `lowerGuildName`, `partition`, `gamer_id`, `guildMemberRequestList`가 함께 포함됩니다. `Backend.NewFunctions.Guild.GetMyGuildInfoV4`의 `GetReturnValueByGuildItem()`을 사용할 경우 이 키들이 길드 메타 데이터(`customGuildInfo`)에 섞여 들어오므로, 메타 데이터를 순회할 때 걸러 주세요.

`_countryCode`는 국가 코드를 등록하지 않은 길드에서 JSON null이 아닌 `"null"` 문자열로 내려옵니다.
:::

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
    public string masterLastLogin;
    public bool? inactivedMaster;
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
        $"masterLastLogin : {masterLastLogin ?? "null"}\n" +
        $"inactivedMaster : {(inactivedMaster.HasValue ? inactivedMaster.Value.ToString() : "null")}\n" +
        viceMasterString;
    }
};
```

```js
public void GetMyGuildInfoV4()
{
    var bro = Backend.Guild.GetMyGuildInfoV4();

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData json = bro.GetReturnValuetoJSON()["result"];

    GuildItem guildItem = new GuildItem();

    guildItem.memberCount = int.Parse(json["memberCount"].ToString());
    guildItem.masterNickname = json["masterNickname"].ToString();
    guildItem.inDate = json["inDate"].ToString();
    guildItem.guildName = json["guildName"].ToString();
    guildItem.goodsCount = int.Parse(json["goodsCount"].ToString());
    if(json.ContainsKey("_immediateRegistration"))
    {
        guildItem._immediateRegistration = bool.Parse(json["_immediateRegistration"].ToString());
    }
    if(json.ContainsKey("_countryCode"))
    {
        guildItem._countryCode = json["_countryCode"].ToString();
    }
    guildItem.masterInDate = json["masterInDate"].ToString();
    // 길드 마스터 자동 교체 기준 일수가 설정되지 않은 경우 두 키는 응답에 존재하지 않습니다.
    if(json.ContainsKey("masterLastLogin") && json["masterLastLogin"] != null)
    {
        guildItem.masterLastLogin = json["masterLastLogin"].ToString();
    }
    if(json.ContainsKey("inactivedMaster") && json["inactivedMaster"] != null)
    {
        guildItem.inactivedMaster = bool.Parse(json["inactivedMaster"].ToString());
    }
    LitJson.JsonData viceListJson = json["viceMasterList"];
    for(int j = 0; j < viceListJson.Count; j++)
    {
        guildItem.viceMasterList.Add(viceListJson[j]["inDate"].ToString(), viceListJson[j]["nickname"].ToString());
    }

    Debug.Log(guildItem.ToString());
}
```

`Backend.NewFunctions.Guild.GetMyGuildInfoV4`를 사용하면 위와 같이 직접 파싱하지 않고 SDK가 제공하는 타입으로 조회할 수 있습니다.

```js
public void GetMyGuildInfoV4WithType()
{
    var bro = Backend.NewFunctions.Guild.GetMyGuildInfoV4();

    if(!bro.IsSuccess())
        return;

    var guildItem = bro.GetReturnValueByGuildItem();

    Debug.Log(guildItem.ToString());
    // 길드 마스터 자동 교체 기준 일수가 설정된 프로젝트에서만 두 필드가 응답에 포함됩니다.
    if(guildItem.hasInactivedMaster && guildItem.inactivedMaster == true)
    {
        Debug.Log($"길드 마스터가 비활성 상태입니다. 마지막 로그인 : {guildItem.masterLastLogin ?? "기록 없음"}");
    }
}
```
