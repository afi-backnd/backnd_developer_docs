---
sidebar_label: "일반 우편 수령"
description: "ReceivePostItem(...)"
---

# ReceivePostItem(...)

public BackendReturnObject **ReceivePostItem**(PostType **postType**, string **postIndate**);

:::caution 주의
우편 수령에 필요한 indate는 우편 불러오기 함수(UPost.GetPostList)를 통해 불러와야합니다.   
우편 불러오기 없이 우편을 수령할 경우 indate가 올바르더라도 수령이 되지 않을 수 있습니다.    
우편 리스트 불러오기를 통해 우편 리스트에 추가된 우편만 수령이 가능합니다.  
:::

## 파라미터

| Value      | Type     | Description                 |
| ---------- | -------- | --------------------------- |
| postType   | PostType | 전체 수령할 우편의 종류     |
| postIndate | string   | 아이템을 받은 우편의 inDate |

### PostType

| Value      | Description                                                                        |
| ---------- | ---------------------------------------------------------------------------------- |
| **Admin**  | **콘솔에서 발송하는 관리자 우편**                                                  |
| **Rank**   | **랭킹 결산 후 자동으로 지급되는 랭킹 우편**                                       |
| **Coupon** | **뒤끝 콘솔의 웹 쿠폰 설정에서 생성한 페이지로 쿠폰을 사용 후 발송되는 쿠폰 우편** |
| **Webshop**| **웹숍에서 아이템 구매 후 발송되는 우편 (뉴플레이에서는 인증 번호 발송 우편도 포함)**                                         |
| User       | 유저끼리 자신의 데이터를 이용하여 발송한 유저 우편                                 |

## 설명

우편의 indate를 이용하여 우편 한 개를 수령합니다.  
  
:::info
**관리자 우편, 쿠폰 우편, 랭킹 우편의 경우 수령 시 리턴값이 동일하므로, 수령 로직을 동일하게 작성해도 무관합니다.**  
  
**유저 우편, 웹숍 우편은 별도의 리턴값이 제공됩니다.**
:::

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- indate가 존재하더라도 PostType이 다를 경우 수령할 수 없습니다.  
- 수령 후, 우편 수신 목록에서 자동으로 삭제되며, returnValue를 통해 어떤 아이템을 수령하는지 확인할 수 있습니다.  
- 해당 우편을 수령한 유저는 콘솔의 - 우편 관리 - 해당 우편 세부정보에서 수령한 날짜와 함께 표시됩니다.(쿠폰, 유저 제외)
- 만료일이 지난 우편은 수령할 수 없습니다.  
- **수령 후 자동으로 유저의 DB에 아이템의 정보가 삽입/수정되지 않으며, 수동으로 클라이언트에서 아이템의 정보를 수령인의 게임 정보에 업데이트하는 과정이 필요합니다.**  

## Example

:::info 웹숍 우편 5.18.6 이전 SDK 버전 안내
5.18.6 이전 버전 SDK에서는 웹숍 우편 처리를 하기 위해서는 아래의 함수를 이용하여 우편 처리를 해야 합니다.

```cs
// 우편 타입을 5로 지정해야 웹숍 우편을 불러올 수 있습니다.
var bro = Backend.NewFunctions.Mail.ReadMail((MailType)5, "2026-01-27T06:57:16.078Z");
```
:::

### 동기

```js
PostType type = PostType.Admin;

//우편 리스트 불러오기
BackendReturnObject bro = Backend.UPost.GetPostList(type, limit);
LitJson.JsonData json = bro.GetReturnValuetoJSON()["postList"];

//우편 리스트중 0번째 우편의 inDate 가져오기
string recentPostIndate = json[0]["inDate"].ToString();

// 동일한 PostType의 우편 수령하기
Backend.UPost.ReceivePostItem(type, recentPostIndate);
```

### 비동기

```js
//우편의 종류
PostType type = PostType.Admin;

//우편 리스트 불러오기
Backend.UPost.GetPostList(type, limit, callback => {
    LitJson.JsonData json = callback.GetReturnValuetoJSON()["postList"];

    //우편 리스트중 0번째 우편의 inDate 가져오기
    string recentPostIndate = json[0]["inDate"].ToString();

    // 우편 수령하기
    Backend.UPost.ReceivePostItem(type, recentPostIndate, callback2 => {
        if(callback2.IsSuccess()) {
            Debug.Log("우편 수령에 성공했습니다.");
        }
    });
});
```

### SendQueue

```js
//우편의 종류
PostType type = PostType.Admin;

//우편 리스트 불러오기
SendQueue.Enqueue(Backend.UPost.GetPostList, type, limit, callback => {
    LitJson.JsonData json = callback.GetReturnValuetoJSON()["postList"];

    //우편 리스트중 0번째 우편의 inDate 가져오기
    string recentPostIndate = json[0]["inDate"].ToString();

    // 동일한 PostType의 우편 수령하기
    SendQueue.Enqueue(Backend.UPost.ReceivePostItem, type, recentPostIndate, callback2 => {
        if(callback2.IsSuccess()) {
            Debug.Log("우편 수령에 성공했습니다.");
        }
    });
});
```

## ReturnCase

### Success cases

**우편 수령에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**존재하지 않는 postIndate를 입력한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : post not found, post을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

### 웹숍 우편의 경우

웹숍 우편의 경우, 아이템 정보가 아래와 같은 형식으로 리턴됩니다. 다른 우편과는 내용이 다르므로 주의하시기 바랍니다.

```js
{
    "postItems": [
        {
            "item": {
                "senderUid": "1769129189338",               // 우편 발송자 UID
                "quantity": 3,                              // 우편에 첨부된 아이템 수량
                "orderId": "MjAyNjAxMjcwNDQ4NTUzNTgxMQ==",  // 우편의 주문 번호
                "price": 1000,                              // 우편에 첨부된 아이템 가격
                "currency": "KRW",                          // 우편에 첨부된 아이템 화폐 단위
                "clientItemId": "100001",                   // 우편에 첨부된 아이템 클라이언트 아이템 ID
                "receiverUid": "1769123876258"              // 우편 수신자 UID
            }
        }
    ]
}
```

### 아이템 종류의 개수가 0개인 우편일 경우

```js
{
    "postItems": []
}
```

### 아이템 종류의 개수가 1개인 우편일 경우

랭킹 우편의 경우, 등록 가능한 아이템이 1개이므로 해당 형식만 리턴됩니다.  

```js
{
    "postItems": [
        {
            "item": {
                "chartFileName": "chartExample.xlsx",
                "itemID": "i101",
                "itemName": "아이템111",
                "hpPower": "1",
                "aabab": "1"
            },
            "itemCount": 1,
            "chartName": "Chart"
        }
    ]
}
```

### 아이템 종류의 개수가 2개 이상의 우편일 경우

```js
{
    "postItems": [
        {
            "item": {
                "chartFileName": "chartExample.xlsx",
                "itemID": "i101",
                "itemName": "아이템111",
                "hpPower": "1",
                "aabab": "1"
            },
            "itemCount": 1,
            "chartName": "Chart"
        },
        {
            "item": {
                "chartFileName": "Weapon.xlsx",
                "itemID": "sword1",
                "itemName": "롱소드",
                "atk": "17",
                "money": "350"
            },
            "itemCount": 2,
            "chartName": "Chart"
        }
    ]
}
```

## Sample Code

```js
public class ReceiveItem {
	public string chartFileName;
	public string itemID;
	public string itemName;
	public int hpPower;
	public int itemCount;

	public override string ToString() {
        return $"chartFileName:{chartFileName}\n" +
        $"itemID:{itemID}\n" +
        $"itemName:{itemName}\n" +
        $"hpPower:{hpPower}\n" +
        $"itemCount:{itemCount}\n";
	}
}
```

```js
public void ReceivePostItemTest() {
	BackendReturnObject bro = Backend.UPost.GetPostList(PostType.Coupon, 100);

	if(bro.IsSuccess()) {
		string postIndate = bro.GetReturnValuetoJSON()["postList"][0]["inDate"].ToString();

		var receiveBro = Backend.UPost.ReceivePostItem(PostType.Coupon, postIndate);

		if(receiveBro.IsSuccess() == false) {
			Debug.LogError("우편 수령중 에러가 발생했습니다. : " + receiveBro);
			return;
		}

		LitJson.JsonData receivePostItemJson = receiveBro.GetReturnValuetoJSON()["postItems"];

		for(int i = 0; i < receivePostItemJson.Count; i++) {
			ReceiveItem item = new ReceiveItem();
			if(receivePostItemJson[i]["item"].ContainsKey("itemName")) {
				item.itemName = receivePostItemJson[i]["item"]["itemName"].ToString();
			}

            // 랭킹 보상의 경우 chartFileName이 존재하지 않습니다.  
			if(receivePostItemJson[i]["item"].ContainsKey("chartFileName")) {
				item.chartFileName = receivePostItemJson[i]["item"]["chartFileName"].ToString();
			}

			if(receivePostItemJson[i]["item"].ContainsKey("itemID")) {
				item.itemID = receivePostItemJson[i]["item"]["itemID"].ToString();
			}

            // 웹숍 우편의 경우 clientItemId로 아이템 ID가 제공됩니다.
            if (receivePostItemJson[i]["item"].ContainsKey("clientItemId")) {
                item.itemID = receivePostItemJson[i]["item"]["clientItemId"].ToString();
            }

			if(receivePostItemJson[i]["item"].ContainsKey("hpPower")) {
				item.hpPower = int.Parse(receivePostItemJson[i]["item"]["hpPower"].ToString());
			}

            if (receivePostItemJson[i].ContainsKey("itemCount")) {
                item.itemCount = int.Parse(receivePostItemJson[i]["itemCount"].ToString());
            }

            // 웹숍 우편의 경우 quantity로 아이템 수량이 제공됩니다.
            if (receivePostItemJson[i]["item"].ContainsKey("quantity")) {
                item.itemCount = int.Parse(receivePostItemJson[i]["item"]["quantity"].ToString());
            }

			Debug.Log(item.ToString());
		}
	}
}
```
