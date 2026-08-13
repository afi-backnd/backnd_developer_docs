---
sidebar_label: "다른 길드 굿즈 정보 조회"
description: "GetGuildGoodsByIndateV3"
---

# GetGuildGoodsByIndateV3

public BackendReturnObject **GetGuildGoodsByIndateV3**(string **guildIndate**);

## 파라미터

| Value       | Type   | Description                   |
| ----------- | ------ | ----------------------------- |
| guildIndate | string | 조회하고자 하는 길드의 inDate |

## 설명

해당 guildIndate를 가진 길드의 굿즈 내역을 조회합니다.  

## Example

### 동기

```js
Backend.Guild.GetGuildGoodsByIndateV3("guildIndate");
```

### 비동기

```js
Backend.Guild.GetGuildGoodsByIndateV3("guildIndate", (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.GetGuildGoodsByIndateV3, "guildIndate", (callback) => {
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

**존재하지 않는 길드 InDate를 호출하는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : guild not found, guild을(를) 찾을 수 없습니다

**InDate를 입력하지 않은 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : Resource not found, Resource을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

```js
{
    // 굿즈 내역
    goods:{
        // 갱신 시각
        updatedAt :
            { S: "2018-11-29T02:03:36.816Z"},
        // 굿즈 1 기부 + 사용 총량
        totalGoods1Amount:
            { N: "0" },
        // 굿즈 1 기부/사용 유저 리스트
        goods1UserList:
            { L: [
                {
                    M:{
                        // 처음으로 굿즈 기부 혹은 사용한 시점
                        inDate:{
                            S: "2018-11-29T02:03:36.816Z"
                        },
                        // 유저 닉네임
                        nickname:{
                            S: "id19"
                        },
                        // 사용한 총 굿즈량(길드 마스터만 존재)
                        usingTotalAmount:{
                            N: "-1"
                        },
                        // 기부한 총 굿즈량
                        totalAmount:{
                            N: "1"
                        },
                        // 마지막 기부 혹은 사용 시점
                        updatedAt: {
                            S: "2019-07-10T07:02:39.223Z"
                        }
                    }
                }
            ] },
        // 굿즈 2 기부 + 사용 총량
        totalGoods2Amount:
            { N: "0" },
        // 굿즈 2 기부/사용 유저 리스트
        goods2UserList:
            { L:[ ] },
        // 굿즈 3 기부 + 사용 총량
        totalGoods3Amount:
            { N: "0" },
        // 굿즈 3 기부/사용 유저 리스트
        goods3UserList:
            { L:[ ] },
        // 굿즈 4 기부 + 사용 총량
        totalGoods4Amount:
            { N: "0" },
        // 굿즈 4 기부/사용 유저 리스트
        goods4UserList:
            { L:[ ] },
        // 굿즈 5 기부 + 사용 총량
        totalGoods5Amount:
            { N: "0" },
        // 굿즈 5 기부/사용 유저 리스트
        goods5UserList:
            { L:[ ] },
        }
    }
}
```

## Sample Code

```js
public class GoodsItem
{
    public int totalGoodsAmount;
    public List<GoodsUserItem> userList = new List<GoodsUserItem>();

    public override string ToString()
    {
        string userString = string.Empty;
        for(int i = 0; i < userList.Count; i++)
        {
            userString += userList[i].ToString() + "\n";
        }

        return $"[totalGoodsAmount : {totalGoodsAmount}]\n" +
        $"{userString}\n";
    }
}

public class GoodsUserItem
{
    public int usingTotalAmount;
    public int totalAmount;
    public string inDate;
    public string nickname;
    public string updatedAt;

    public override string ToString()
    {
        return $"\tnickname : {nickname}\n" +
        $"\tinDate : {inDate}\n" +
        $"\ttotalAmount : {totalAmount}\n" +
        $"\tusingTotalAmount : {usingTotalAmount}\n" +
        $"\tupdatedAt : {updatedAt}\n";
    }
}
```

```js
public void GetGuildGoodsByIndateV3()
{
    var bro = Backend.Guild.GetGuildGoodsByIndateV3("2021-12-16T08:51:00.706Z");

    if(!bro.IsSuccess())
        return;

    Dictionary<string, GoodsItem> goodsDictionary = new Dictionary<string, GoodsItem>();

    var goodsJson = bro.GetFlattenJSON()["goods"];
    foreach(var column in goodsJson.Keys)
    {
        if(column.Contains("totalGoods"))
        {
            GoodsItem goodsItem = new GoodsItem();

            goodsItem.totalGoodsAmount = int.Parse(goodsJson[column].ToString());

            string goodsNum = column.Replace("totalGoods", "");
            goodsNum = goodsNum.Replace("Amount", "");

            string goodsName = "goods" + goodsNum + "UserList";

            LitJson.JsonData userListJson = goodsJson[goodsName];
            for(int i = 0; i < userListJson.Count; i++)
            {
                GoodsUserItem user = new GoodsUserItem();

                user.inDate = userListJson[i]["inDate"].ToString();
                user.nickname = userListJson[i]["nickname"].ToString();
                if(userListJson[i].ContainsKey("usingTotalAmount"))
                {
                    user.usingTotalAmount = int.Parse(userListJson[i]["usingTotalAmount"].ToString());
                }
                user.totalAmount = int.Parse(userListJson[i]["totalAmount"].ToString());
                user.updatedAt = userListJson[i]["updatedAt"].ToString();

                goodsItem.userList.Add(user);
            }
            goodsDictionary.Add("goods" + goodsNum, goodsItem);
        }
    }

    foreach(var dic in goodsDictionary)
    {
        Debug.Log($"-----{dic.Key}------\n" +
        $"{dic.Value.ToString()}\n");
    }
}
```
