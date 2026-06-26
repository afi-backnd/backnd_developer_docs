---
sidebar_label: 관리자 우편 불러오기
description: "관리자 우편 불러오기"
---

# GetMails(MailType.Admin)

public Task< GetMailsResult > **GetMailsAsync**(MailType**Admin** **mailType**);  
public Task< GetMailsResult > **GetMailsAsync**(MailType**Admin** **mailType**, int **limit**);

## 파라미터

| Value    | Type     | Description                                                                     |
| -------- | -------- | ------------------------------------------------------------------------------- |
| mailType | MailType | 불러올 우편의 종류                                                              |
| limit    | int      | 불러올 우편의 갯수. 최소 10개, 최대 100개. 미입력 시 limit은 10개로 지정됩니다. |

> limit이 10 미만일 경우, 10으로 고정됩니다.  

### MailType

| Value     | Description                                                                    |
| --------- | ------------------------------------------------------------------------------ |
| **Admin** | **콘솔에서 발송하는 관리자 우편**                                              |
| Rank      | 랭킹 결산 후 자동으로 지급되는 랭킹 우편                                       |
| Coupon    | 뒤끝 콘솔의 웹 쿠폰 설정에서 생성한 페이지로 쿠폰을 사용 후 발송되는 쿠폰 우편 |
| User      | 유저끼리 자신의 데이터를 이용하여 발송한 유저 우편                             |

## 설명

우편 종류(MailType)에 따른 우편 리스트를 불러옵니다.  

각 MailType의 종류마다 리턴값에 추가된 값이 존재하거나 없을 수 있으므로 아래 ReturnValueJson을 참고해주시기 바랍니다.  

> 유저 우편의 경우, 관리자 우편과 랭킹 우편과는 Json 타입이 크게 다를 수 있으므로 주의해주시기 바랍니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- 각 우편은 우편 종류별로 만료일이 존재하며, 만료일이 지난 우편은 리스트에서 삭제됩니다.  
- 우편 불러오기를 통해 해당 우편을 불러온 유저는 콘솔-우편 관리의 세부 정보에서 '미수령'으로 표시되며, 수령 시 수령한 날짜가 표시됩니다.  
- 관리자 우편의 경우 아이템 수령 시 아이템이 자동으로 삭제됩니다.  
- 우편 만료일은 콘솔에서 설정하실 수 있습니다.(하루 이내 - 1시간, 3시간, 6시간, 12시간, 24시간, 7일, 15일, 30일)

### 다국어 지원

- 유저의 국가 코드가 존재하고, 콘솔에 해당 국가 우편이 등록되어 있는 경우 → 해당 국가의 우편 제목과 내용을 리턴합니다.  
- 유저의 국가 코드가 존재하지 않거나, 콘솔에 해당 국가 이벤트가 등록되어 있지 않은 경우 → 기본 설정의 제목과 내용을 리턴합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndMail.Instance.GetMailsAsync(MailType.Admin, 10);
var infoList = reqResult.AsAdminResult().GetInfoList();

for (int i = 0; i < infoList.Count; i++)
{
    Debug.Log("제목 : " + infoList[i].Title);
    Debug.Log("inDate : " + infoList[i].InDate);
}
```

### Callback 방식

```js
BackndMail.Instance.GetMails(MailType.Admin, 10, callback =>
{
   var infoList = reqResult.AsAdminResult().GetInfoList();

   for (int i = 0; i < infoList.Count; i++)
	{
			Debug.Log("제목 : " + infoList[i].Title);
			Debug.Log("inDate : " + infoList[i].InDate);
	}
});
```


## ReturnCase

### Success cases

**우편 불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

**불러올 우편이 없을 경우**  
statusCode : 200  
message : Success  
returnValue : { "postList" : []}

## ReturnValueJson

### 관리자 우편

관리자 우편의 경우, 아이템이 0개인 우편, 아이템이 1개인 우편, 아이템이 2개 이상인 우편이 존재하므로 주의하여 파싱해주시기 바랍니다.  

```js
{
    "postList": [
        {
            "content": "내용", // 콘솔에서 보낸 우편의 내용
            "expirationDate": "2021-12-21T10:32:11.021Z", //우편이 만료되는 날짜
            "reservationDate": "2021-12-20T10:32:11.021Z", // 우편을 등록한 날짜
            "nickname": "나는야유저1", // 보내는 이의 닉네임
            "inDate": "2021-12-20T10:32:11.021Z", // 우편의 inDate
            "title": "아이템0개", // 우편의 제목
            "author": "관리자", // 우편의 발송인
            "sentDate": "2021-12-20T10:32:11.021Z", // 보낸 날짜
            "items": [] // 우편에 첨부된 아이템(해당 우편의 첨부된 아이템의 수는 0개입니다.)
        },
        {
            "content": "아이템이 1개인 우편입니다",
            "expirationDate": "2021-12-21T10:33:27.711Z",
            "reservationDate": "2021-12-20T10:33:27.711Z",
            "nickname": "나는야유저1",
            "inDate": "2021-12-20T10:33:27.711Z",
            "title": "아이템 1개만 보냅니다",
            "author": "GM런처",
            "sentDate": "2021-12-20T10:33:27.711Z",
            "items": [ // 아이템 정보
                {
                    "item": {
                        "chartFileName": "아이템 차트 파일.xlsx",
                        "itemID": "i101",
                        "itemName": "아이템1",
                        "hpPower": "1"
                    },
                    "itemCount": 1,
                    "chartName": "Chart"
                }
            ]
        },
        {
            "content": "아이템이 3개인 우편입니다",
            "expirationDate": "2021-12-21T10:32:52.615Z",
            "reservationDate": "2021-12-20T10:32:52.615Z",
            "nickname": "나는야유저1",
            "inDate": "2021-12-20T10:32:52.615Z",
            "title": "아이템 3개 보냅니다",
            "author": "GM 뒤끝",
            "sentDate": "2021-12-20T10:32:52.615Z",
            "items": [
                {
                    "item": {
                        "chartFileName": "아이템 차트 파일.xlsx",
                        "itemID": "i101",
                        "itemName": "아이템1",
                        "hpPower": "1"
                    },
                    "itemCount": 100,
                    "chartName": "Chart"
                },
                {
                    "item": {
                        "chartFileName": "아이템 차트 파일.xlsx",
                        "itemID": "i102",
                        "itemName": "아이템2",
                        "hpPower": "2"
                    },
                    "itemCount": 20000
                },
                {
                    "item": {
                        "chartFileName": "아이템 차트 파일.xlsx",
                        "itemID": "i103",
                        "itemName": "아이템303",
                        "hpPower": "3"
                    },
                    "itemCount": 3,
                    "chartName": "Chart"
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
public async Task GetPostListTest()
{
    int limit = 100;
    var postType = MailType.Admin;
    var getResult = await BackndMail.Instance.GetMailsAsync(postType, limit);
    if (!getResult.IsSuccess())
    {
        Debug.LogError(getResult.ToString());
        return;
    }

    var infoList = getResult.AsAdminResult().GetInfoList();
    List<UPostItem> postItemList = new List<UPostItem>();

    for (int i = 0; i < infoList.Count; i++)
    {
        var info = infoList[i];

        UPostItem postItem = new UPostItem();
        postItem.inDate = info.InDate;
        postItem.title = info.Title;
        postItem.postType = postType;
        postItem.content = info.Content;
        postItem.expirationDate = DateTime.Parse(info.ExpirationDate);
        postItem.reservationDate = DateTime.Parse(info.ReservationDate);
        postItem.nickname = info.Nickname;
        postItem.sentDate = DateTime.Parse(info.SentDate);
        postItem.author = info.Author;

        if (info.ItemList.Count > 0)
        {
            for (int itemNum = 0; itemNum < info.ItemList.Count; itemNum++)
            {
                var itemInfo = info.ItemList[itemNum];

                UPostChartItem item = new UPostChartItem();
                item.itemCount = itemInfo.ItemCount;
                item.chartFileName = itemInfo.TableName;
                item.itemID = itemInfo.ItemFields["itemID"].ToString();
                item.itemName = itemInfo.ItemFields["itemName"].ToString();
                item.hpPower = itemInfo.ItemFields["hpPower"].ToString();

                postItem.items.Add(item);
            }
        }

        postItemList.Add(postItem);
        Debug.Log(postItem.ToString());
    }
}
```
