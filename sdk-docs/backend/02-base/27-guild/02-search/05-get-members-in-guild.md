---
sidebar_label: "길드원 리스트 조회"
description: "GetGuildMemberListV3"
---

# GetGuildMemberListV3

public BackendReturnObject **GetGuildMemberListV3**(string **guildIndate**);  
public BackendReturnObject **GetGuildMemberListV3**(string **guildIndate**, int **limit**);  
public BackendReturnObject **GetGuildMemberListV3**(string **guildIndate**, string **firstKey**);  
public BackendReturnObject **GetGuildMemberListV3**(string **guildIndate**, int **limit**, string **firstKey**);

## 파라미터

| Value       | Type   | Description                                         | default |
| ----------- | ------ | --------------------------------------------------- | :-----: |
| guildIndate | string | 길드 멤버 리스트를 불러올 길드의 inDate             |    -    |
| limit       | int    | (Optional) 불러올 길드 멤버 리스트의 수             |   100   |
| firstKey    | string | (Optional) 불러올 길드 멤버 리스트의 시작점(offset) |    -    |


## 설명

해당 guildIndate를 가진 길드의 멤버 리스트를 조회합니다.  
* limit이 0 이하일 경우 100으로 호출됩니다.


## Example

### 동기

```js
Backend.Guild.GetGuildMemberListV3("guildIndate");
Backend.Guild.GetGuildMemberListV3("guildIndate", 5);

// 처음 리스트를 읽을 때, firstkey 값을 저장해 놓은 뒤,
BackendReturnObject bro = Backend.Guild.GetGuildMemberListV3();
String firstKey = bro.FirstKeyString();
// 다음 페이지 넘어갈 때 firstKey를 offset으로 하여 다시 읽어옴
Backend.Guild.GetGuildMemberListV3("guildIndate", firstKey);
Backend.Guild.GetGuildMemberListV3("guildIndate", 5, firstKey);
```

### 비동기

```js
Backend.Guild.GetGuildMemberListV3("guildIndate", (callback) => {
  // 이후 처리
});

Backend.Guild.GetGuildMemberListV3("guildIndate", 5, (callback) => {
  // 이후 처리
});

// 동기 함수와 같은 방식으로 firstKey를 저장한 후, 다음 리스트를 불러옴
Backend.Guild.GetGuildMemberListV3("guildIndate", firstKey, (callback) => {
  // 이후 처리
});

Backend.Guild.GetGuildMemberListV3("guildIndate", 20, firstKey, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.GetGuildMemberListV3, "guildIndate", (callback) => {
    // 이후 처리
});

SendQueue.Enqueue(Backend.Guild.GetGuildMemberListV3, "guildIndate", 5, (callback) => {
    // 이후 처리
});

// 동기 함수와 같은 방식으로 firstKey를 저장한 후, 다음 리스트를 불러옴
SendQueue.Enqueue(Backend.Guild.GetGuildMemberListV3, "guildIndate", firstKey, (callback) => {
    // 이후 처리
});

SendQueue.Enqueue(Backend.Guild.GetGuildMemberListV3, "guildIndate", 20, firstKey, (callback) => {
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

**존재하지 않는 guildIndate일 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

```js
{
    rows:
    [
        // 길드원 1
        {
            // 길드원 1의 굿즈 2 기부(+사용) 총량
            totalGoods2Amount:{ N:"21" },
            // 길드원 1의 굿즈 3 기부(+사용) 총량
            totalGoods3Amount:{ N:"8" },
            // 길드원 1 닉네임
            nickname:{ S:"id2" },
            // 길드원 1의 길드 indate(길드 가입 시점)
            inDate:{ S:"2018-11-21T05:42:24.511Z" },
            // 길드원 1 indate
            gamerInDate:{ S:"2018-11-08T03:32:18.210Z" },
            // 길드원 1 길드 권한(master/viceMaster/member)
            position:{ S:"master" },
            //마지막으로 접속한 날짜
            lastLogin:{S:"2021-07-23T07:42:01.019Z"}
        },
        // 길드원 2
        {
            totalGoods2Amount:{ N:"0" },
            totalGoods3Amount:{ N:"0" },
            inDate:{ S:"2018-11-22T05:45:11.638Z" },
            gamerInDate:{ S:"2018-11-13T01:46:25.323Z" },
            totalGoods1Amount:{ N:"0" },
            position:{ S:"member" },
            lastLogin:{S:"2021-07-22T01:49:01.019Z"}
        },
    ],
   //firstKey가 존재할 경우
   firstKey:{"inDate":{"S":"2021-07-15T01:18:02.841Z"}}
   //firstKey가 존재하지 않을 경우
   firstKey:null
}
```

## Sample Code

```js
public class GuildMemberItem
{
    public string nickname;
    public string inDate;
    public string gamerInDate;
    public string position;
    public string lastLogin;
    public Dictionary<string, int> totalGoodsAmount = new Dictionary<string, int>();
    public override string ToString()
    {
        string goodsString = string.Empty;
        foreach(var dic in totalGoodsAmount)
        {
            goodsString += $"{dic.Key} : {dic.Value}\n";
        }
        return $"nickname : {nickname}\n" +
        $"inDate : {inDate}\n" +
        $"gamerInDate : {gamerInDate}\n" +
        $"position : {position}\n" +
        $"lastLogin : {lastLogin}\n" +
        goodsString;
    }
};
```

```js
public void GetGuildMemberListV3()
{
    var bro = Backend.Guild.GetGuildMemberListV3("2021-10-29T06:32:10.147Z");

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData guildMemberJson = bro.FlattenRows();

    List<GuildMemberItem> guildMemberList = new List<GuildMemberItem>();

    for(int i = 0; i < guildMemberJson.Count; i++)
    {
        GuildMemberItem member = new GuildMemberItem();

        member.nickname = guildMemberJson[i]["nickname"].ToString();
        member.inDate = guildMemberJson[i]["inDate"].ToString();
        member.gamerInDate = guildMemberJson[i]["gamerInDate"].ToString();
        member.lastLogin = guildMemberJson[i]["lastLogin"].ToString();
        member.position = guildMemberJson[i]["position"].ToString();
        foreach(var goods in guildMemberJson[i].Keys)
        {
            if(goods.Contains("totalGoods"))
            {
                member.totalGoodsAmount.Add(goods, int.Parse(guildMemberJson[i][goods].ToString()));
            }
        }

        guildMemberList.Add(member);
        Debug.Log(member.ToString());
    }
}
```
