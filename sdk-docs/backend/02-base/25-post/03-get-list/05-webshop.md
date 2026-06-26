---
sidebar_label: 웹숍 우편 불러오기
sidebar_position: 3.1
---

# GetPostList(PostType.Webshop)

public BackendReturnObject **GetPostList**(PostType.**Webshop**);  
public BackendReturnObject **GetPostList**(PostType.**Webshop**, int **limit**);

## 파라미터

| Value    | Type     | Description                                                                     |
| -------- | -------- | ------------------------------------------------------------------------------- |
| postType | PostType | 불러올 우편의 종류                                                              |
| limit    | int      | 불러올 우편의 갯수. 최소 10개, 최대 100개. 미입력 시 limit은 10개로 지정됩니다. |

> limit이 10 미만일 경우, 10으로 고정됩니다.  

### PostType

| Value     | Description                                                                    |
| --------- | ------------------------------------------------------------------------------ |
| Admin | 콘솔에서 발송하는 관리자 우편                                          |
| Rank      | 랭킹 결산 후 자동으로 지급되는 랭킹 우편                                       |
| Coupon    | 뒤끝 콘솔의 웹 쿠폰 설정에서 생성한 페이지로 쿠폰을 사용 후 발송되는 쿠폰 우편 |
| **Webshop**  | **웹숍에서 아이템 구매 후 발송되는 우편 (뉴플레이에서는 인증 번호 발송 우편도 포함)** |
| User      | 유저끼리 자신의 데이터를 이용하여 발송한 유저 우편                             |

## 설명

우편 종류(PostType)에 따른 우편 리스트를 불러옵니다.  

각 PostType의 종류마다 리턴값에 추가된 값이 존재하거나 없을 수 있으므로 아래 GetReturnValuetoJSON()을 참고해주시기 바랍니다.  

> 유저 우편의 경우, 관리자 우편과 랭킹 우편과는 Json 타입이 크게 다를 수 있으므로 주의해주시기 바랍니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- 각 우편은 우편 종류별로 만료일이 존재하며, 만료일이 지난 우편은 리스트에서 삭제됩니다.  
- 우편 불러오기를 통해 해당 우편을 불러온 유저는 콘솔-우편 관리의 세부 정보에서 '미수령'으로 표시되며, 수령 시 수령한 날짜가 표시됩니다.  
- 웹숍 우편의 경우 아이템 수령 시 아이템이 자동으로 삭제됩니다.  
- 웹숍 우편의 만료일은 30일로 고정되어 있습니다.

### 지원 웹숍 종류

- Newplay Store
- Xsolla Web Shop

## Example

:::info 5.18.6 이전 SDK 버전 안내
5.18.6 이전 버전 SDK에서는 웹숍 우편 처리를 하기 위해서는 아래의 함수를 이용하여 우편 처리를 해야 합니다.

```cs
// 우편 타입을 5로 지정해야 웹숍 우편을 불러올 수 있습니다.
var bro = Backend.NewFunctions.Mail.GetMailList((MailType)5);
```
:::

### 동기

```js
BackendReturnObject bro = Backend.UPost.GetPostList(PostType.Webshop, 10);
LitJson.JsonData json = bro.GetReturnValuetoJSON()["postItems"];

for(int i = 0; i < json.Count; i++)  {
    Debug.Log("제목 : " +  json[i]["title"].ToString());
    Debug.Log("inDate : " +  json[i]["inDate"].ToString());
}
```

### 비동기

```js
Backend.UPost.GetPostList(PostType.Webshop, 10, callback => {
    LitJson.JsonData json = callback.GetReturnValuetoJSON()["postItems"];

    for(int i = 0; i < json.Count; i++)  {
        Debug.Log("제목 : " +  json[i]["title"].ToString());
        Debug.Log("inDate : " +  json[i]["inDate"].ToString());
    }
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.UPost.GetPostList, PostType.Webshop, 10, callback =>  {
    LitJson.JsonData json = callback.GetReturnValuetoJSON()["postItems"];

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
returnValue : { "postItems" : []}

## GetReturnValuetoJSON

### 뉴플레이 웹숍 우편

```js
{
    "postItems": [
        {
            "inDate": "2026-01-27T06:57:16.078Z",           // 우편의 inDate
            "title": "상품 1 구매 완료",                       // 우편의 제목
            "content": "아이템을 구매해주셔서 감사합니다.",         // 우편의 내용
            "expirationDate": "2026-02-26T06:57:16.078Z",   // 만료 날짜
            "webshop": "newplay",                           // 우편이 발송된 웹숍 서비스 이름
            "buyer": "1769129189338",                       // 우편의 구매자
            "author": "NEWPLAY",                            // 우편의 발송인
            "orderId": "MjAyNjAxMjcwNDQ4NTUzNTgxMQ==",      // 우편의 주문 번호
            "itemId": "아이템1",                              // 우편의 아이템 ID
            "item": {                                       // 우편에 첨부된 아이템 정보
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

### 뉴플레이 인증 번호 발송 우편

```js
{
    "postItems": [
        {
            "inDate": "2024-06-10T12:34:56.789Z",           // 우편의 inDate
            "title": "인증 번호 발송",                         // 우편의 제목
            "content": "L33MXO",                            // 우편의 내용
            "expirationDate": "2024-07-10T12:34:56.789Z",   // 만료 날짜
            "webshop": "newplay",                           // 우편이 발송된 웹숍 서비스 이름
            "author": "NEWPLAY",                            // 우편의 발송인
        }
    ]
}
```

### 엑솔라 웹숍 우편

```js
{
    "postItems": [
        {
            "inDate": "2025-11-06T05:27:54.283Z",               // 우편의 inDate
            "title": "아이템 구매 보상",                            // 우편의 제목
            "content": "202605220025",                          // 우편의 내용
            "expirationDate": "2053-03-23T05:27:54.283Z",       // 만료 날짜
            "webshop": "Xsolla",                                // 우편이 발송된 웹숍 서비스 이름
            "buyer": "5cb725f0-5574-11f1-98f6-4b40b988c7ae",    // 우편의 구매자
            "author": "엑솔라",                                  // 우편의 발송인
            "items": [                                          // 우편에 첨부된 아이템 정보
                {
                    "orderId": "202605220025",                                  // 주문 번호
                    "type": "virtual_good",                                     // 아이템 유형
                    "price": 3000,                                              // 결제 총액
                    "currency": "PHP",                                          // 화폐 단위
                    "amount": 2000,                                             // 아이템 단일 가격
                    "receiverUid": "5cb725f0-5574-11f1-98f6-4b40b988c7ae",      // 우편 수신자 UID
                    "senderUid": "5cb725f0-5574-11f1-98f6-4b40b988c7ae",        // 우편 발송자 UID
                    "is_pre_order": false,                                      // 사전 주문 여부
                    "quantity": 1,                                              // 아이템 수량
                    "sku": "I.tem_0-01",                                        // 엑솔라 SKU (아이템 식별자)
                    "promotions": []                                            // 적용된 프로모션 목록
                },
                {
                    "orderId": "202605220025",
                    "type": "virtual_currency",
                    "price": 3000,
                    "currency": "PHP",
                    "amount": 1000,
                    "receiverUid": "5cb725f0-5574-11f1-98f6-4b40b988c7ae",
                    "senderUid": "5cb725f0-5574-11f1-98f6-4b40b988c7ae",
                    "is_pre_order": false,
                    "quantity": 2,
                    "sku": "GOLD_1000",
                    "promotions": [                                             // 적용된 프로모션 목록
                        {
                            "amount_without_discount": "6000",                  // 할인 전 금액
                            "amount_with_discount": "5000",                     // 할인 후 금액
                            "sequence": 1                                       // 프로모션 순번
                        },
                        {
                            "amount_without_discount": "5000",
                            "amount_with_discount": "4000",
                            "sequence": 2
                        }
                    ]
                }
            ]
        }
    ]
}
```

> 엑솔라 웹숍 우편은 뉴플레이 웹숍 우편과 달리 `items` 배열로 다수의 아이템 정보가 전달되며, 각 아이템은 SKU·프로모션·아이템 타입 등 엑솔라 고유 필드를 포함합니다.

:::info 아이템 유형
virtual_good / virtual_currency / game_key / bundle / value_point
:::   

## Sample Code

웹숍 우편은 제공 서비스(`webshop` 필드)에 따라 JSON 구조가 다릅니다.

- 뉴플레이 아이템 구매 우편: `item` 단일 객체로 구매 정보 전달
- 뉴플레이 인증 번호 발송 우편: 아이템 정보 없이 `content`에 인증 번호만 포함
- 엑솔라 아이템 구매 우편: `items` 배열로 다수의 아이템 및 프로모션 정보 전달

아래 샘플 코드는 세 종류의 우편을 모두 분기 처리합니다.

```csharp
// 뉴플레이 웹숍 구매 아이템 정보
public class WebshopItem {
    public string senderUid;
    public int quantity;
    public string orderId;
    public int price;
    public string currency;
    public string clientItemId;
    public string receiverUid;

    public override string ToString() {
        return
        "item : \n" +
        $"| senderUid : {senderUid}\n" +
        $"| quantity : {quantity}\n" +
        $"| orderId : {orderId}\n" +
        $"| price : {price}\n" +
        $"| currency : {currency}\n" +
        $"| clientItemId : {clientItemId}\n" +
        $"| receiverUid : {receiverUid}\n";
    }
}

// 엑솔라 프로모션 정보
public class XsollaPromotion {
    public string amountWithoutDiscount;
    public string amountWithDiscount;
    public int sequence;

    public override string ToString() {
        return
        $"  - sequence : {sequence}, " +
        $"without discount : {amountWithoutDiscount}, " +
        $"with discount : {amountWithDiscount}\n";
    }
}

// 엑솔라 웹숍 구매 아이템 정보
public class XsollaItem {
    public string orderId;
    public string type;          // 아이템 유형 - virtual_good / virtual_currency / game_key / bundle / value_point
    public int price;
    public int amount;
    public string receiverUid;
    public string senderUid;
    public bool isPreOrder;
    public int quantity;
    public string sku;
    public string currency;
    public List<XsollaPromotion> promotions = new List<XsollaPromotion>();

    public override string ToString() {
        string promotionStr = "";
        foreach(var promo in promotions) {
            promotionStr += promo.ToString();
        }
        return
        "item : \n" +
        $"| orderId : {orderId}\n" +
        $"| type : {type}\n" +
        $"| price : {price}\n" +
        $"| amount : {amount}\n" +
        $"| receiverUid : {receiverUid}\n" +
        $"| senderUid : {senderUid}\n" +
        $"| is_pre_order : {isPreOrder}\n" +
        $"| quantity : {quantity}\n" +
        $"| sku : {sku}\n" +
        $"| currency : {currency}\n" +
        $"| promotions : \n{(string.IsNullOrEmpty(promotionStr) ? "  (none)\n" : promotionStr)}";
    }
}

public enum WebshopPostType {
    NewplayVerificationCode, // 뉴플레이 인증 번호 발송 우편
    NewplayItemPurchase,     // 뉴플레이 아이템 구매 우편
    XsollaItemPurchase       // 엑솔라 아이템 구매 우편
}

// 웹숍 우편 정보
public class WebshopPostItem {
    public WebshopPostType postType;
    public string inDate;
    public string title;
    public string content;
    public DateTime expirationDate;
    public string webshop;
    public string buyer;
    public string author;
    public string orderId;
    public string itemId;
    public WebshopItem item;            // 뉴플레이 아이템 구매 우편
    public List<XsollaItem> items;      // 엑솔라 아이템 구매 우편

    public override string ToString() {
        string result =
        $"title : {title}\n" +
        $"inDate : {inDate}\n" +
        $"content : {content}\n" +
        $"expirationDate : {expirationDate}\n" +
        $"webshop : {webshop}\n" +
        $"author : {author}\n";

        switch(postType) {
            case WebshopPostType.NewplayItemPurchase:
                result += $"buyer : {buyer}\n";
                result += $"orderId : {orderId}\n";
                result += $"itemId : {itemId}\n";
                result += item != null ? item.ToString() : "item : null\n";
                break;
            case WebshopPostType.XsollaItemPurchase:
                result += $"buyer : {buyer}\n";
                if(items != null) {
                    foreach(var xitem in items) {
                        result += xitem.ToString();
                    }
                }
                break;
            case WebshopPostType.NewplayVerificationCode:
                // 인증 번호는 content 필드로 전달됨
                break;
        }

        return result;
    }
}
```

```csharp
public void GetWebshopPostListTest() {
    int limit = 100;

    BackendReturnObject bro = Backend.UPost.GetPostList(PostType.Webshop, limit);
    if(!bro.IsSuccess()) {
        Debug.LogError(bro.ToString());
        return;
    }

    LitJson.JsonData postItemsJson = bro.GetReturnValuetoJSON()["postItems"];

    List<WebshopPostItem> postItems = new List<WebshopPostItem>();

    for(int i = 0; i < postItemsJson.Count; i++) {
        WebshopPostItem postItem = new WebshopPostItem();

        postItem.inDate = postItemsJson[i]["inDate"].ToString();
        postItem.title = postItemsJson[i]["title"].ToString();
        postItem.content = postItemsJson[i]["content"].ToString();
        postItem.expirationDate = DateTime.Parse(postItemsJson[i]["expirationDate"].ToString());
        postItem.webshop = postItemsJson[i]["webshop"].ToString();
        postItem.author = postItemsJson[i]["author"].ToString();

        // webshop 값으로 우편 종류를 분기
        if(postItem.webshop == "Xsolla") {
            // 엑솔라 아이템 구매 우편: items 배열
            postItem.postType = WebshopPostType.XsollaItemPurchase;
            postItem.buyer = postItemsJson[i]["buyer"].ToString();
            postItem.items = new List<XsollaItem>();

            if(postItemsJson[i].ContainsKey("items") && postItemsJson[i]["items"].IsArray) {
                LitJson.JsonData itemsJson = postItemsJson[i]["items"];
                for(int j = 0; j < itemsJson.Count; j++) {
                    XsollaItem xitem = new XsollaItem();
                    xitem.orderId = itemsJson[j]["orderId"].ToString();
                    xitem.type = itemsJson[j]["type"].ToString();
                    xitem.price = int.Parse(itemsJson[j]["price"].ToString());
                    xitem.amount = int.Parse(itemsJson[j]["amount"].ToString());
                    xitem.receiverUid = itemsJson[j]["receiverUid"].ToString();
                    xitem.senderUid = itemsJson[j]["senderUid"].ToString();
                    xitem.isPreOrder = bool.Parse(itemsJson[j]["is_pre_order"].ToString());
                    xitem.quantity = int.Parse(itemsJson[j]["quantity"].ToString());
                    xitem.sku = itemsJson[j]["sku"].ToString();
                    xitem.currency = itemsJson[j]["currency"].ToString();

                    if(itemsJson[j].ContainsKey("promotions") && itemsJson[j]["promotions"].IsArray) {
                        LitJson.JsonData promotionsJson = itemsJson[j]["promotions"];
                        for(int k = 0; k < promotionsJson.Count; k++) {
                            XsollaPromotion promo = new XsollaPromotion();
                            promo.amountWithoutDiscount = promotionsJson[k]["amount_without_discount"].ToString();
                            promo.amountWithDiscount = promotionsJson[k]["amount_with_discount"].ToString();
                            promo.sequence = int.Parse(promotionsJson[k]["sequence"].ToString());
                            xitem.promotions.Add(promo);
                        }
                    }

                    postItem.items.Add(xitem);
                }
            }
        } else {
            // 뉴플레이 우편: item 단일 객체 또는 인증 번호 우편
            if(postItemsJson[i].ContainsKey("item") && postItemsJson[i]["item"] != null) {
                // 뉴플레이 아이템 구매 우편
                postItem.postType = WebshopPostType.NewplayItemPurchase;

                postItem.buyer = postItemsJson[i]["buyer"].ToString();
                postItem.orderId = postItemsJson[i]["orderId"].ToString();
                postItem.itemId = postItemsJson[i]["itemId"].ToString();

                WebshopItem item = new WebshopItem();

                item.senderUid = postItemsJson[i]["item"]["senderUid"].ToString();
                item.quantity = int.Parse(postItemsJson[i]["item"]["quantity"].ToString());
                item.orderId = postItemsJson[i]["item"]["orderId"].ToString();
                item.price = int.Parse(postItemsJson[i]["item"]["price"].ToString());
                item.currency = postItemsJson[i]["item"]["currency"].ToString();
                item.clientItemId = postItemsJson[i]["item"]["clientItemId"].ToString();
                item.receiverUid = postItemsJson[i]["item"]["receiverUid"].ToString();

                postItem.item = item;
            } else {
                // 뉴플레이 인증 번호 발송 우편의 경우 item이 없음
                postItem.postType = WebshopPostType.NewplayVerificationCode;
            }
        }

        postItems.Add(postItem);
        Debug.Log(postItem.ToString());
    }
}
```
