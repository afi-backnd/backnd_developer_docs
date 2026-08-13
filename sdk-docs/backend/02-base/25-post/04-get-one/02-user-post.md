---
sidebar_label: "유저 우편 수령"
description: "ReceivePostItem(User)"
---

# ReceivePostItem(User)

public BackendReturnObject **ReceivePostItem**(PostType **postType**, string **postIndate**);

## 파라미터

| Value      | Type     | Description                 |
| ---------- | -------- | --------------------------- |
| postType   | PostType | 전체 수령할 우편의 종류     |
| postIndate | string   | 아이템을 받은 우편의 inDate |

### PostType

| Value    | Description                                            |
| -------- | ------------------------------------------------------ |
| Admin    | 콘솔에서 발송하는 관리자 우편                          |
| Rank     | 랭킹 결산 후 자동으로 지급되는 랭킹 우편               |
| Coupon   | 쿠폰 전용 웹에서 쿠폰 코드를 입력하여 받는 우편        |
| Webshop  | 웹숍에서 아이템 구매 후 발송되는 우편 (뉴플레이에서는 인증 번호 발송 우편도 포함)      |
| **User** | **유저끼리 자신의 데이터를 이용하여 발송한 유저 우편** |

## 설명

우편의 indate를 이용하여 우편 한 개를 수령합니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- indate가 존재하더라도 PostType이 다를 경우 수령할 수 없습니다.  
- 수령 후에 우편이 **자동으로 삭제되지 않으며**, DeleteUserPost 함수를 호출할 경우에만 삭제됩니다.  
- 만료일이 지난 우편은 수령할 수 없습니다.  
- **수령 후 자동으로 유저의 DB에 아이템의 정보가 삽입/수정되지 않으며, 수동으로 클라이언트에서 아이템의 정보를 수령인의 게임 정보에 업데이트하는 과정이 필요합니다.**  

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
PostType type = PostType.User;

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
PostType type = PostType.User;

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
PostType type = PostType.User;

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

```js
{
    "postItems": [
        {
            "key1": "아이템1",
            "key2": "아이템2",
            "key3": "아이템3"
        }
    ]
}
```

**삽입한 데이터**  

```js
Param param = new Param();
Dictionary<string, string> dic = new Dictionary<string, string>();

dic.Add("key1", "아이템1");
dic.Add("key2", "아이템2");
dic.Add("key3", "아이템3");

param.Add(columnName, dic);

string rowIndate = Backend.GameData.Insert(tableName, param).GetInDate();

PostItem postItem = new PostItem {
    Title = "우편 제목",
    Content = "우편 내용",
    TableName = tableName,
    RowInDate = rowIndate,
    Column = columnName
};

Backend.UPost.SendUserPost(userIndate, postItem);
```

### Sample Code

```js
void ReceivePostOneInternal(PostType postType, BackendReturnObject bro) {
    if(bro.IsSuccess() == false) {
        Debug.LogError($"우편 수령하기에 실패했습니다. : " + bro);
        return;
    }

    StringBuilder stringBuilder = new StringBuilder();

    try {
        foreach(var postItemKey in bro.GetReturnValuetoJSON()["postItems"].Keys) {
            stringBuilder.AppendLine($"{postItemKey} : {postJson[postItemKey].ToString()}");
        }
    }
    catch(Exception e) {
        Debug.LogError("우편의 아이템 파싱 도중 에러가 발생했습니다. : " + e);
    }

    Debug.Log(stringBuilder.ToString());
}
```
