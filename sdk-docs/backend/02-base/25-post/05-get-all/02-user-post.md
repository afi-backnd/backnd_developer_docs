---
sidebar_label: 유저 우편 모두 수령
---

# ReceivePostItemAll(User)

public BackendReturnObject **ReceivePostItemAll**(PostType **postType**);

## 파라미터

| Value    | Type     | Description             |
| -------- | -------- | ----------------------- |
| postType | PostType | 전체 수령할 우편의 종류 |

### PostType

| Value    | Description                                            |
| -------- | ------------------------------------------------------ |
| Admin    | 콘솔에서 발송하는 관리자 우편                          |
| Rank     | 랭킹 결산 후 자동으로 지급되는 랭킹 우편               |
| Coupon   | 쿠폰 전용 웹에서 쿠폰 코드를 입력하여 받는 우편        |
| Webshop  | 웹숍에서 아이템 구매 후 발송되는 우편 (뉴플레이에서는 인증 번호 발송 우편도 포함)      |
| **User** | **유저끼리 자신의 데이터를 이용하여 발송한 유저 우편** |

## 설명

현재까지 조회된 모든 우편들을 수령합니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- 수령할 우편이 존재하지 않을 경우 에러가 발생합니다.  

## Example

### 동기

```js
var bro = Backend.UPost.ReceivePostItemAll(PostType.User);

// 모두 수령에 대한 로직은 'Sample Code'를 참고해주세요
```

### 비동기

```js
Backend.UPost.ReceivePostItemAll(PostType.User, (callback) => {
  // 모두 수령에 대한 로직은 'Sample Code'를 참고해주세요
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.UPost.ReceivePostItemAll, PostType.User, (callback) => {
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

### 유저 우편

```js
{
    "postItems": [
        {
            "key1": "아이템1",
            "key2": "아이템2",
            "key3": "아이템3"
        },
        {
            "key1": "아이템4",
            "key2": "아이템5",
            "key3": "아이템6"
        },
        {
            "key1": "아이템7",
            "key2": "아이템8",
            "key3": "아이템9"
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

PostItem postItem = new PostItem
{
    Title = "우편 제목",
    Content = "우편 내용",
    TableName = tableName,
    RowInDate = rowIndate,
    Column = columnName
};

Backend.UPost.SendUserPost(userIndate, postItem);
```

## Sample Code

```js
public void ReceivePostItemAllTest() {
	BackendReturnObject bro = Backend.UPost.GetPostList(PostType.Coupon, 100);

	if(bro.IsSuccess() == false) {
		Debug.Log("우편을 불러오는 중 에러가 발생했습니다.");
		return;
	}

	var receiveBro = Backend.UPost.ReceivePostItemAll(PostType.Coupon);

	if(receiveBro.IsSuccess() == false) {
		Debug.LogError("우편 모두 수령하기 중 에러가 발생하였습니다. : " + receiveBro);
		return;
	}

    System.Text.StringBuilder stringBuilder = new System.Text.StringBuilder();

	foreach(LitJson.JsonData postItemJson in receiveBro.GetReturnValuetoJSON()["postItems"]) {
        foreach(string postItemKey in postItemJson.Keys) {
            stringBuilder.AppendLine($"{postItemKey} : {postItemJson[postItemKey].ToString()}");
        }
        stringBuilder.AppendLine();
	}

	Debug.Log(stringBuilder.ToString());
}
```
