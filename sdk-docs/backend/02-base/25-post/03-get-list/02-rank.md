---
sidebar_label: "랭킹 우편 불러오기"
description: "GetPostList(PostType.Rank)"
---

# GetPostList(PostType.Rank)

public BackendReturnObject **GetPostList**(PostType.**Rank**);  
public BackendReturnObject **GetPostList**(PostType.**Rank**, int **limit**);

## 파라미터

| Value    | Type     | Description                                                                     |
| -------- | -------- | ------------------------------------------------------------------------------- |
| postType | PostType | 불러올 우편의 종류                                                              |
| limit    | int      | 불러올 우편의 갯수. 최소 10개, 최대 100개. 미입력 시 limit은 10개로 지정됩니다. |

> limit이 10 미만일 경우, 10으로 고정됩니다.  

### PostType

| Value    | Description                                                                    |
| -------- | ------------------------------------------------------------------------------ |
| Admin    | 콘솔에서 발송하는 관리자 우편                                                  |
| **Rank** | **랭킹 결산 후 자동으로 지급되는 랭킹 우편**                                   |
| Coupon   | 뒤끝 콘솔의 웹 쿠폰 설정에서 생성한 페이지로 쿠폰을 사용 후 발송되는 쿠폰 우편 |
| Webshop  | 웹숍에서 아이템 구매 후 발송되는 우편 (뉴플레이에서는 인증 번호 발송 우편도 포함) |
| User     | 유저끼리 자신의 데이터를 이용하여 발송한 유저 우편                             |

## 설명

우편 종류(PostType)에 따른 우편 리스트를 불러옵니다.  

각 PostType의 종류마다 리턴값에 추가된 값이 존재하거나 없을 수 있으므로 아래 GetReturnValuetoJSON()을 참고해주시기 바랍니다.  

> 유저 우편의 경우, 관리자 우편과 랭킹 우편과는 Json 타입이 크게 다를 수 있으므로 주의해주시기 바랍니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- 각 우편은 우편 종류별로 만료일이 존재하며, 만료일이 지난 우편은 리스트에서 삭제됩니다.  
- 우편 불러오기를 통해 해당 우편을 불러온 유저는 콘솔-우편 관리의 세부 정보에서 '미수령'으로 표시되며, 수령 시 수령한 날짜가 표시됩니다.  
- 랭킹 우편의 경우 아이템 수령 시 아이템이 자동으로 삭제됩니다.  
- 우편 만료일은 우편이 발송된 시간에서 7일 후입니다.  

### 랭킹 우편 제목

- 랭킹 우편 제목 생성 규칙은 [랭킹 보상 확인](/guide/console-guide/backnd-base/post/reward-rank) 문서를 참고해 주세요.  

### 길드 랭킹 우편 보상

- 길드 랭킹 보상이 발송된 이후에 우편을 확인하지 않고 길드를 탈퇴할 경우, 해당 유저는 탈퇴한 길드에 대한 길드 랭킹 보상을 받을 수 없으며 재가입을 하더라도 우편 확인이 불가능합니다. GetPostList()호출 시 길드에 가입이 되어있어야만 해당 길드에 대한 랭킹 보상 우편을 받을 수 있습니다.(보상 지급 후에 가입할 경우에는 제외)

## Example

### 동기

```js
BackendReturnObject bro = Backend.UPost.GetPostList(PostType.Rank, 10);
LitJson.JsonData json = bro.GetReturnValuetoJSON()["postList"];

for(int i = 0; i < json.Count; i++)  {
    Debug.Log("제목 : " +  json[i]["title"].ToString());
    Debug.Log("inDate : " +  json[i]["inDate"].ToString());
}
```

### 비동기

```js
Backend.UPost.GetPostList(PostType.Rank, 10, callback => {
    LitJson.JsonData json = callback.GetReturnValuetoJSON()["postList"];

    for(int i = 0; i < json.Count; i++)  {
        Debug.Log("제목 : " +  json[i]["title"].ToString());
        Debug.Log("inDate : " +  json[i]["inDate"].ToString());
    }
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.UPost.GetPostList, PostType.Rank, 10, callback =>  {
    LitJson.JsonData json = callback.GetReturnValuetoJSON()["postList"];

    for(int i = 0; i < json.Count; i++)  {
        Debug.Log("제목 : " +  json[i]["title"].ToString());
        Debug.Log("inDate : " +  json[i]["inDate"].ToString());
    }
});
```

## ReturnCase

### Success cases

**우편 불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**불러올 우편이 없을 경우**  
statusCode : 200  
message : Success  
returnValue : { "postList" : []}

## GetReturnValuetoJSON

### 랭킹 우편

랭킹 우편은 아이템이 1개인 경우만 존재합니다. 다만 접근은 관리자 우편과 동일하게 배열 형태로 접근할 수 있습니다.  

```js
{
    "postList": [
        {
            "rankType": "user", // 랭킹 타입 - guild : 길드, user : 유저
            "content": "최고 스코어 랭크(1-1)", // 랭킹 보상의 내용
            "expirationDate": "2021-12-26T19:03:52.243Z", // 만료 날짜
            "reservationDate": "2021-12-19T19:03:52.243Z", // 우편을 등록한 날짜
            "nickname": "유저2", // 받을 유저
            "inDate": "2021-12-19T19:03:52.243Z", // 우편의 inDate
            "title": "최고 스코어 랭크(1-1)", // 랭킹 보상 우편의 제목
            "postTitle": {
                "ko": "최고 스코어 랭크를 달성했습니다!",
                "en": "You Best Score!",
                "fallback": "en"
            },
						"sentDate": "2021-12-19T19:03:52.243Z", // 보낸 날짜
            "items": [
                {
                    "item": {
                        "itemID": "i104",
                        "itemName": "아이템4",
                    },
                    "itemCount": 999999,
                    "chartName": "랭크 보상 차트"
                }
            ]
        },
        {
            "rankType": "guild",
            "content": "무작위 총력전 랭킹(21 - 40)",
            "expirationDate": "2021-12-26T19:03:44.163Z",
            "reservationDate": "2021-12-19T19:03:44.163Z",
            "nickname": "유저2",
            "inDate": "2021-12-19T19:03:44.163Z",
            "title": "무작위 총력전 랭킹(21 - 40)",
            "postTitle": {
                "ko": "무작위 총력전 랭킹",
                "en": "Rank",
                "fallback": "en"
            },
						"sentDate": "2021-12-19T19:03:44.163Z",
            "items": [
                {
                    "item": {
                        "itemID": "i102",
                        "itemName": "아이템2",
                        "hpPower": "10",
                        "aabab": "2"
                    },
                    "itemCount": 1,
                    "chartName": "랭크 보상 차트2"
                }
            ]
        }
    ]
}
```

## Sample Code

해당 예제는 관리자 우편, 랭킹 우편, 쿠폰 우편에서 공통적으로 사용가능합니다.  

```js
public class UPostChartItem {
	public string chartFileName;
	public string itemID;
	public string itemName;
	public string hpPower;
	public int itemCount;

	public override string ToString() {
		return
		"item : \n" +
		$"| chartFileName : {chartFileName}\n" +
		$"| itemID : {itemID}\n" +
		$"| itemName : {itemName}\n" +
		$"| itemCount : {itemCount}\n" +
		$"| hpPower : {hpPower}\n";
	}
}

public class UPostItem {
	public PostType postType;
	public string title;
	public string content;
	public DateTime expirationDate;
	public DateTime reservationDate;
	public DateTime sentDate;
	public string nickname;
	public string inDate;
	public string author; // 관리자 우편만 존재합니다.  
	public string rankType; // 랭킹 우편만 존재합니다.  
	public List<UPostChartItem> items = new List<UPostChartItem>();

	public override string ToString() {
		string totalString =
		$"title : {title}\n" +
		$"inDate : {inDate}\n";
		if(postType == PostType.Admin || postType == PostType.Rank) {
			totalString +=
			$"content : {content}\n" +
			$"expirationDate : {expirationDate}\n" +
			$"reservationDate : {reservationDate}\n" +
			$"sentDate : {sentDate}\n" +
			$"nickname : {nickname}\n";

			if(postType == PostType.Admin) {
				totalString += $"author : {author}\n";
			}

			if(postType == PostType.Rank) {
				totalString += $"rankType : {rankType}\n";
			}
		}

		string itemList = string.Empty;
		for(int i = 0; i < items.Count; i++) {
			itemList += items[i].ToString();
			itemList += "\n";
		}

		totalString += itemList;

		return totalString;
	}
}
```

```js
public void GetPostListTest() {
	int limit = 100;
	PostType postType = PostType.Rank;

	BackendReturnObject bro = Backend.UPost.GetPostList(postType, limit);
	if(!bro.IsSuccess()) {
		Debug.LogError(bro.ToString());
		return;
	}

	LitJson.JsonData postListJson = bro.GetReturnValuetoJSON()["postList"];

	List<UPostItem> postItemList = new List<UPostItem>();

	for(int i = 0; i < postListJson.Count; i++) {
		UPostItem postItem = new UPostItem();

		postItem.inDate = postListJson[i]["inDate"].ToString();
		postItem.title = postListJson[i]["title"].ToString();
		postItem.postType = postType;

		if(postType == PostType.Admin || postType == PostType.Rank) {
			postItem.content = postListJson[i]["content"].ToString();
			postItem.expirationDate = DateTime.Parse(postListJson[i]["expirationDate"].ToString());
			postItem.reservationDate = DateTime.Parse(postListJson[i]["reservationDate"].ToString());
			postItem.nickname = postListJson[i]["nickname"]?.ToString();
			postItem.sentDate = DateTime.Parse(postListJson[i]["sentDate"].ToString());

			if(postListJson[i].ContainsKey("author")) {
				postItem.author = postListJson[i]["author"].ToString();
			}

			if(postListJson[i].ContainsKey("rankType")) {
				postItem.author = postListJson[i]["rankType"].ToString();
			}
		}

		if(postListJson[i]["items"].Count > 0) {
			for(int itemNum = 0; itemNum < postListJson[i]["items"].Count; itemNum++) {

				UPostChartItem item = new UPostChartItem();
				item.itemCount = int.Parse(postListJson[i]["items"][itemNum]["itemCount"].ToString());
				item.chartFileName = postListJson[i]["items"][itemNum]["item"]["chartFileName"].ToString();
				item.itemID = postListJson[i]["items"][itemNum]["item"]["itemID"].ToString();
				item.itemName = postListJson[i]["items"][itemNum]["item"]["itemName"].ToString();
				item.hpPower = postListJson[i]["items"][itemNum]["item"]["hpPower"].ToString();

				postItem.items.Add(item);
			}
		}

		postItemList.Add(postItem);

		Debug.Log(postItem.ToString());
	}
}
```
