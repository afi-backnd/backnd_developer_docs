---
sidebar_label: "모든 길드 리스트 조회"
description: "GetGuildListV4"
---

# GetGuildListV4

public BackendReturnObject **GetGuildListV4**();  
public BackendReturnObject **GetGuildListV4**(int **limit**);  
public BackendReturnObject **GetGuildListV4**(string **firstKey**);  
public BackendReturnObject **GetGuildListV4**(int **limit**, string **firstKey**);

## 파라미터

| Value    | Type   | Description                                    | default |
| -------- | ------ | ---------------------------------------------- | :-----: |
| limit    | int    | (Optional) 불러올 길드 리스트의 수             |   100   |
| firstKey | string | (Optional) 불러올 길드 리스트의 시작점(offset) |    -    |

## 설명

모든 길드 리스트를 조회합니다.  
길드 리스트에는 각 길드의 메타 데이터가 포함되어 리턴됩니다.  
* limit이 0 이하일 경우 100으로 호출됩니다. limit이 100을 초과할 경우에도 100으로 제한됩니다.

:::caution GetGuildListV3에서 변경된 점
* 반환값이 일반 JSON으로 정규화되어 내려옵니다. 따라서 `FlattenRows()`가 아닌 `Rows()`로 바로 읽을 수 있습니다.
* 뒤끝 콘솔에 길드 마스터 자동 교체 기준 일수가 설정된 경우, 각 길드에 `masterLastLogin`과 `inactivedMaster` 키가 추가됩니다. 자세한 내용은 [길드 마스터 자동 교체 신청](../05-guild-master/10-claim-master.md)을 참고해 주세요.
* `GetGuildListV3`는 deprecated 되었습니다. 신규 개발은 `GetGuildListV4`를 사용해 주세요.
:::

## Example

### 동기

```js
Backend.Guild.GetGuildListV4();
Backend.Guild.GetGuildListV4(5);

// 처음 리스트를 읽을 때, firstkey 값을 저장해 놓은 뒤,
BackendReturnObject bro = Backend.Guild.GetGuildListV4();
string firstKey = bro.FirstKeystring();
// 다음 페이지 넘어갈 때 firstKey를 offset으로 하여 다시 읽어옴
Backend.Guild.GetGuildListV4(firstKey);
Backend.Guild.GetGuildListV4(5, firstKey);
```

### 비동기

```js
Backend.Guild.GetGuildListV4((callback) => {
  // 이후 처리
});
Backend.Guild.GetGuildListV4(5, (callback) => {
  // 이후 처리
});

// 동기 함수와 같은 방식으로 firstKey를 저장한 후, 다음 리스트를 불러옴
Backend.Guild.GetGuildListV4(firstKey, (callback) => {
  // 이후 처리
});
Backend.Guild.GetGuildListV4(20, firstKey, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.GetGuildListV4, (callback) => {
  // 이후 처리
});
SendQueue.Enqueue(Backend.Guild.GetGuildListV4, 5, (callback) => {
  // 이후 처리
});

// 동기 함수와 같은 방식으로 firstKey를 저장한 후, 다음 리스트를 불러옴
SendQueue.Enqueue(Backend.Guild.GetGuildListV4, firstKey, (callback) => {
  // 이후 처리
});
SendQueue.Enqueue(Backend.Guild.GetGuildListV4, 20, firstKey, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

```js
{
    rows:
    [
        {
            // 길드의 inDate
            inDate: "2019-04-12T07:38:16.522Z",
            // 길드명
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
            // 그룹 정보(그룹을 사용하는 길드에만 존재)
            propertyGroup: "KR-01",
            propertyGroupUuid: "019d8abc-a966-7d7a-b0ac-479a293527a1",
            // 길드 마스터의 마지막 로그인 시각(길드 마스터 자동 교체 기준 일수가 설정된 경우에만 존재, 로그인 기록이 없으면 null)
            masterLastLogin: "2026-08-10T12:30:00.000Z",
            // 길드 마스터의 비활성 여부(길드 마스터 자동 교체 기준 일수가 설정된 경우에만 존재, 판단에 실패하면 null)
            inactivedMaster: false
        }
    ],
    firstKey: { // 리스트를 페이징 처리하기 위한 offset key(리스트의 끝까지 불러온 경우 리턴되지 않음)
        inDate: "2018-07-13T07:04:39.650Z"
    }
}
```

:::caution row별로 키 구성이 다릅니다
길드 리스트의 각 row는 해당 길드가 저장한 값만 포함합니다. 국가 코드를 등록하지 않은 길드에는 `_countryCode`가 `"null"` 문자열로 내려오고, 그룹을 사용하지 않는 길드에는 `propertyGroup`·`propertyGroupUuid`가 없으며, `SetRegistrationValueV3`를 호출하지 않은 길드에는 `_immediateRegistration`이 없습니다. 반환값을 직접 파싱할 때는 아래 Sample Code처럼 `ContainsKey`로 확인한 후 사용해 주세요.
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
public void GetGuildListV4()
{
    var bro = Backend.Guild.GetGuildListV4();

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData json = bro.Rows();

    List<GuildItem> guildList = new List<GuildItem>();

    for(int i = 0; i < json.Count; i++)
    {
        GuildItem guildItem = new GuildItem();

        guildItem.memberCount = int.Parse(json[i]["memberCount"].ToString());
        guildItem.masterNickname = json[i]["masterNickname"].ToString();
        guildItem.inDate = json[i]["inDate"].ToString();
        guildItem.guildName = json[i]["guildName"].ToString();
        if(json[i].ContainsKey("goodsCount"))
        {
            guildItem.goodsCount = int.Parse(json[i]["goodsCount"].ToString());
        }
        if(json[i].ContainsKey("_immediateRegistration"))
        {
            guildItem._immediateRegistration = bool.Parse(json[i]["_immediateRegistration"].ToString());
        }
        if(json[i].ContainsKey("_countryCode"))
        {
            guildItem._countryCode = json[i]["_countryCode"].ToString();
        }
        guildItem.masterInDate = json[i]["masterInDate"].ToString();
        // 길드 마스터 자동 교체 기준 일수가 설정되지 않은 경우 두 키는 응답에 존재하지 않습니다.
        if(json[i].ContainsKey("masterLastLogin") && json[i]["masterLastLogin"] != null)
        {
            guildItem.masterLastLogin = json[i]["masterLastLogin"].ToString();
        }
        if(json[i].ContainsKey("inactivedMaster") && json[i]["inactivedMaster"] != null)
        {
            guildItem.inactivedMaster = bool.Parse(json[i]["inactivedMaster"].ToString());
        }
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

`Backend.NewFunctions.Guild.GetAllGuildListV4`를 사용하면 위와 같이 직접 파싱하지 않고 SDK가 제공하는 타입으로 조회할 수 있습니다.

```js
public void GetAllGuildListV4()
{
    var bro = Backend.NewFunctions.Guild.GetAllGuildListV4();

    if(!bro.IsSuccess())
        return;

    foreach(var guildItem in bro.GetReturnValueByGuildList())
    {
        Debug.Log(guildItem.ToString());
    }
}
```
