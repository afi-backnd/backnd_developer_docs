---
sidebar_label: 관리자, 랭킹 보상, 쿠폰 우편 모두 수령
description: "관리자, 랭킹 보상, 쿠폰 우편 모두 수령"
---

# ReceiveAllMails(Admin, Rank, Coupon)

public Task< ReceiveMailResult > **ReceiveAllMailsAsync**(MailType **mailType**);

:::caution 주의
우편 모두 수령하기는 우편 리스트에 등록된 우편만 수령할 수 있습니다.  

함수 이용 전에 우편 리스트 불러오기(GetMails)를 통해 우편 리스트를 갱신하는 것을 권장드립니다.  
:::

## 파라미터

| Value    | Type     | Description             |
| -------- | -------- | ----------------------- |
| mailType | MailType | 전체 수령할 우편의 종류 |

### MailType

| Value      | Description                                                                        |
| ---------- | ---------------------------------------------------------------------------------- |
| **Admin**  | **콘솔에서 발송하는 관리자 우편**                                                  |
| **Rank**   | **랭킹 결산 후 자동으로 지급되는 랭킹 우편**                                       |
| **Coupon** | **뒤끝 콘솔의 웹 쿠폰 설정에서 생성한 페이지로 쿠폰을 사용 후 발송되는 쿠폰 우편** |
| User       | 유저끼리 자신의 데이터를 이용하여 발송한 유저 우편                                 |

## 설명

mailType 별로 현재까지 조회한 모든 우편을 수령합니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- **관리자 우편, 쿠폰 우편, 랭킹 우편의 경우 수령 시 리턴값이 동일하므로, 수령 로직을 동일하게 작성해도 무관합니다.**  
- 수령할 우편이 존재하지 않을 경우 에러가 발생합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndMail.Instance.ReceiveAllMailsAsync(MailType.Admin);

// 모두 수령에 대한 로직은 'Sample Code'를 참고해주세요
```

### Callback 방식

```js
BackndMail.Instance.ReceiveAllMails(MailType.Admin, (callback) =>
{
  // 모두 수령에 대한 로직은 'Sample Code'를 참고해주세요
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

### Error cases

**더 이상 수령할 우편이 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : post not found, post을(를) 찾을 수 없습니다

## ReturnValueJson

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
public async Task ReceivePostItemAllTest()
{
    var reqResult = await BackndMail.Instance.GetMailsAsync(MailType.Admin, 100);
    if (reqResult.IsSuccess() == false)
    {
        Debug.Log("우편을 불러오는 중 에러가 발생했습니다.");
        return;
    }

    var receiveResult = await BackndMail.Instance.ReceiveAllMailsAsync(MailType.Admin);
    if (receiveResult.IsSuccess() == false)
    {
        Debug.LogError("우편 모두 수령하기 중 에러가 발생하였습니다. : " + receiveResult);
        return;
    }

    var infoList = receiveResult.AsSystemResult().GetItemList();
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
```
