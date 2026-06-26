---
sidebar_label: 관리자, 랭킹 보상, 쿠폰 우편 하나 수령
description: "관리자, 랭킹 보상, 쿠폰 우편 하나 수령"
---

# ReceiveMail(Admin, Rank, Coupon)

public Task< ReceiveMailResult > **ReceiveMailAsync**(MailType **mailType**, string **mailInDate**);

:::caution 주의
우편 수령에 필요한 indate는 우편 불러오기 함수(GetMails)를 통해 불러와야합니다.   
우편 불러오기 없이 우편을 수령할 경우 indate가 올바르더라도 수령이 되지 않을 수 있습니다.    
우편 리스트 불러오기를 통해 우편 리스트에 추가된 우편만 수령이 가능합니다.  
:::

## 파라미터

| Value      | Type     | Description                 |
| ---------- | -------- | --------------------------- |
| mailType   | MailType | 전체 수령할 우편의 종류     |
| mailInDate | string   | 아이템을 받은 우편의 inDate |

### MailType

| Value      | Description                                                                        |
| ---------- | ---------------------------------------------------------------------------------- |
| **Admin**  | **콘솔에서 발송하는 관리자 우편**                                                  |
| **Rank**   | **랭킹 결산 후 자동으로 지급되는 랭킹 우편**                                       |
| **Coupon** | **뒤끝 콘솔의 웹 쿠폰 설정에서 생성한 페이지로 쿠폰을 사용 후 발송되는 쿠폰 우편** |
| User       | 유저끼리 자신의 데이터를 이용하여 발송한 유저 우편                                 |

## 설명

우편의 indate를 이용하여 우편 한 개를 수령합니다.  
**관리자 우편, 쿠폰 우편, 랭킹 우편의 경우 수령 시 리턴값이 동일하므로, 수령 로직을 동일하게 작성해도 무관합니다.**  
유저 우편은 별도의 리턴값이 제공됩니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- indate가 존재하더라도 PostType이 다를 경우 수령할 수 없습니다.  
- 수령 후, 우편 수신 목록에서 자동으로 삭제되며, returnValue를 통해 어떤 아이템을 수령하는지 확인할 수 있습니다.  
- 해당 우편을 수령한 유저는 콘솔의 - 우편 관리 - 해당 우편 세부정보에서 수령한 날짜와 함께 표시됩니다.(쿠폰, 유저 제외)
- 만료일이 지난 우편은 수령할 수 없습니다.  
- **수령 후 자동으로 유저의 DB에 아이템의 정보가 삽입/수정되지 않으며, 수동으로 클라이언트에서 아이템의 정보를 수령인의 게임 정보에 업데이트하는 과정이 필요합니다.**  

## Example

### Task 방식

```js
var type = MailType.Admin;

//우편 리스트 불러오기
var listResult = await BackndMail.Instance.GetMailsAsync(type, limit);
//우편 리스트중 0번째 우편의 inDate 가져오기
var recentPostIndate = listResult.AsAdminResult().GetInfo(0).InDate;

// 동일한 PostType의 우편 수령하기
var receiveResult = await BackndMail.Instance.ReceiveMailAsync(type, recentPostIndate);
```

### Callback 방식

```js
//우편의 종류
var type = MailType.Admin;

//우편 리스트 불러오기
BackndMail.Instance.GetMails(type, limit, callback =>
{
    //우편 리스트중 0번째 우편의 inDate 가져오기
    var recentPostIndate = listResult.AsAdminResult().GetInfo(0).InDate;

    // 우편 수령하기
    BackndMail.Instance.ReceiveMail(type, recentPostIndate, callback2 =>
    {
        if(callback2.IsSuccess())
        {
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
returnValue : ReturnValueJson 참조

### Error cases

**존재하지 않는 postIndate를 입력한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : post not found, post을(를) 찾을 수 없습니다

## ReturnValueJson

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
                    "itemCount": 2
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
public async Task ReceivePostItemTest()
{
    var reqResult = await BackndMail.Instance.GetMailsAsync(MailType.Coupon, 100);
    if (reqResult.IsSuccess())
    {
        var postIndate = reqResult.AsCouponResult().GetInfo(0).InDate;
        var receiveResult = await BackndMail.Instance.ReceiveMailAsync(MailType.Coupon, postIndate);
        if (receiveResult.IsSuccess() == false)
        {
            Debug.LogError("우편 수령중 에러가 발생했습니다. : " + receiveResult);
            return;
        }

        var infoList = receiveResult.GetItemList().GetItemList();
        for (int i = 0; i < infoList.Count; i++)
        {
            var info = infoList[i];                

            ReceiveItem item = new ReceiveItem();
            if (info.ItemFields.ContainsKey("itemName"))
            {
                item.itemName = info.ItemFields["itemName"].ToString();
            }

            // 랭킹 보상의 경우 chartFileName이 존재하지 않습니다.  
            if (info.ItemFields.ContainsKey("chartFileName"))
            {
                item.chartFileName = info.ItemFields["chartFileName"].ToString();
            }

            if (info.ItemFields.ContainsKey("itemID"))
            {
                item.itemID = info.ItemFields["itemID"].ToString();
            }

            if (info.ItemFields.ContainsKey("hpPower"))
            {
                item.hpPower = int.Parse(info.ItemFields["hpPower"].ToString());
            }

            item.itemCount = info.ItemCount;

            Debug.Log(item.ToString());
        }
    }
}
```
