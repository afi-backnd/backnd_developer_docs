---
sidebar_label: 내 길드 굿즈 정보 조회
description: "내 길드 굿즈 정보 조회"
---

# GetUserGuildGoodsAsync

public Task< GetGuildGoodsResult > **GetUserGuildGoodsAsync**();

## 설명

내가 속한 길드의 굿즈 내역을 조회합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndGuild.Instance.GetUserGuildGoodsAsync();
```

### Callback 방식
```js
BackndGuild.Instance.GetUserGuildGoods((callback) =>
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

**길드에 속하지 않은 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : notGuildMember 사전 조건을 만족하지 않습니다.  

## ReturnValueJson

```js
{
    // 굿즈 내역
    goods:{
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
       	admin: // 콘솔에서 수정한 굿즈 증감 내역
	{M:
		{ goods1: // 굿즈1의 수정 내역
			{M:
				{
				 "usingTotalAmount":{"N":"-100"},
				 "totalAmount":{"N":"1000"},
				 "updatedAt":{"S":"2021-07-26T03:12:16.317Z"}
				}
			}
		}
		{ goods2: // 굿즈2의 수정 내역
			{M:
				{
				 "usingTotalAmount":{"N":"-300"},
				 "totalAmount":{"N":"50"},
				 "updatedAt":{"S":"2021-07-22T01:14:33.426Z"}
				}
			}
		}
	},
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
public async Task GetMyGuildGoodsV3()
{
    var reqResult = await BackndGuild.Instance.GetUserGuildGoodsAsync();
    if (!reqResult.IsSuccess())
        return;

    var goodsDictionary = new Dictionary<string, GoodsItem>();
    var goodsInfoList = reqResult.GetInfoList();
        //var goodsJson = reqResult.GetFlattenJSON()["goods"];
    foreach (var info in goodsInfoList)
    {
        GoodsItem goodsItem = new GoodsItem();
        goodsItem.totalGoodsAmount = info.TotalAmount;

        var userInfoList = info.UserList;
        for (int i = 0; i < userInfoList.Count; i++)
        {
            var userInfo = userInfoList[i];

            GoodsUserItem user = new GoodsUserItem();
            user.inDate = userInfo.InDate;
            user.nickname = userInfo.Nickname;
            user.usingTotalAmount = int.Parse(userInfo.UsageAmount);                
            user.totalAmount = int.Parse(userInfo.TotalAmount);
            user.updatedAt = userInfo.UpdatedAt;

            goodsItem.userList.Add(user);
        }
        goodsDictionary.Add("goods" + info.Number, goodsItem);
    }

    foreach (var dic in goodsDictionary)
    {
        Debug.Log($"-----{dic.Key}------\n" +
        $"{dic.Value.ToString()}\n");
    }
}
```
