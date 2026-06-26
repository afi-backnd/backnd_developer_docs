---
sidebar_label: 일반 우편 모두 수령
---

# ReceivePostItemAll(...)

public BackendReturnObject **ReceivePostItemAll**(PostType **postType**);

:::caution 주의
우편 모두 수령하기는 우편 리스트에 등록된 우편만 수령할 수 있습니다.  

함수 이용 전에 우편 리스트 불러오기(UPost.GetPostList)를 통해 우편 리스트를 갱신하는 것을 권장드립니다.  
:::

## 파라미터

| Value    | Type     | Description             |
| -------- | -------- | ----------------------- |
| postType | PostType | 전체 수령할 우편의 종류 |

### PostType

| Value      | Description                                                                        |
| ---------- | ---------------------------------------------------------------------------------- |
| **Admin**  | **콘솔에서 발송하는 관리자 우편**                                                  |
| **Rank**   | **랭킹 결산 후 자동으로 지급되는 랭킹 우편**                                       |
| **Coupon** | **뒤끝 콘솔의 웹 쿠폰 설정에서 생성한 페이지로 쿠폰을 사용 후 발송되는 쿠폰 우편** |
| **Webshop**| **웹숍에서 아이템 구매 후 발송되는 우편 (뉴플레이에서는 인증 번호 발송 우편도 포함)**                                         |
| User       | 유저끼리 자신의 데이터를 이용하여 발송한 유저 우편                                 |

## 설명

postType 별로 현재까지 조회한 모든 우편을 수령합니다.  

:::info
**관리자 우편, 쿠폰 우편, 랭킹 우편의 경우 수령 시 리턴값이 동일하므로, 수령 로직을 동일하게 작성해도 무관합니다.**  
  
**유저 우편, 웹숍 우편은 별도의 리턴값이 제공됩니다.**
:::

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- 수령할 우편이 존재하지 않을 경우 에러가 발생합니다.  

## Example

:::info 웹숍 우편 5.18.6 이전 SDK 버전 안내
5.18.6 이전 버전 SDK에서는 웹숍 우편 처리를 하기 위해서는 아래의 함수를 이용하여 우편 처리를 해야 합니다.

```cs
// 우편 타입을 5로 지정해야 웹숍 우편을 불러올 수 있습니다.
var bro = Backend.NewFunctions.Mail.ReadAllMail((MailType)5);
```
:::

### 동기

```js
var bro = Backend.UPost.ReceivePostItemAll(PostType.Admin);

// 모두 수령에 대한 로직은 'Sample Code'를 참고해주세요
```

### 비동기

```js
Backend.UPost.ReceivePostItemAll(PostType.Admin, (callback) => {
  // 모두 수령에 대한 로직은 'Sample Code'를 참고해주세요
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.UPost.ReceivePostItemAll, PostType.Admin, (callback) => {
    // 모두 수령에 대한 로직은 'Sample Code'를 참고해주세요
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**더 이상 수령할 우편이 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : post not found, post을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

```js
{
    "postItems": [
        [], // 첨부된 아이템이 없을 경우
        [
            {
                "item": { // 첨부된 아이템이 하나일 경우(랭킹 우편은 아이템이 1개밖에 등록되지 않습니다.)
                    "chartFileName": "chartExample.xlsx",
                    "itemID": "i101",
                    "itemName": "아이템111",
                    "hpPower": "1",
                    "aabab": "1"
                },
                "itemCount": 1,
                "chartName": "Chart"
            }
        ],
        [
            {
                "item": { // 첨부된 아이템이 여러개일 경우
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
                    "chartFileName": "itemTable.xlsx",
                    "itemID": "item-2392",
                    "itemName": "롱소드"
                },
                "itemCount": 1,
                "chartName": "아이템"
            }
        ]
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
public void ReceivePostItemAllTest() {

	BackendReturnObject bro = Backend.UPost.GetPostList(PostType.Admin, 100);

	if(bro.IsSuccess() == false) {
		Debug.Log("우편을 불러오는 중 에러가 발생했습니다.");
		return;
	}

	var receiveBro = Backend.UPost.ReceivePostItemAll(PostType.Admin);

	if(receiveBro.IsSuccess() == false) {
		Debug.LogError("우편 모두 수령하기 중 에러가 발생하였습니다. : " + receiveBro);
		return;
	}

	foreach(LitJson.JsonData postItemJson in receiveBro.GetReturnValuetoJSON()["postItems"]) {

		for(int j = 0; j < postItemJson.Count; j++) {
			if(!postItemJson[j].ContainsKey("item")) {
				continue;
			}

			ReceiveItem item = new ReceiveItem();
			if(postItemJson[j]["item"].ContainsKey("itemName")) {
				item.itemName = postItemJson[j]["item"]["itemName"].ToString();
			}

			// 랭킹 보상의 경우 chartFileName이 존재하지 않습니다.  
			if(postItemJson[j]["item"].ContainsKey("chartFileName")) {
				item.chartFileName = postItemJson[j]["item"]["chartFileName"].ToString();
			}

			if(postItemJson[j]["item"].ContainsKey("itemID")) {
				item.itemID = postItemJson[j]["item"]["itemID"].ToString();
			}

			if(postItemJson[j]["item"].ContainsKey("hpPower")) {
				item.hpPower = int.Parse(postItemJson[j]["item"]["hpPower"].ToString());
			}

			if(postItemJson[j].ContainsKey("itemCount")) {
				item.itemCount = int.Parse(postItemJson[j]["itemCount"].ToString());
			}

			Debug.Log(item.ToString());
		}
	}
}
```
