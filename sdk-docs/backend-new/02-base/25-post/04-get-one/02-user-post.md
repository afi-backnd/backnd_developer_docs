---
sidebar_label: 유저 우편 하나 수령
---

# ReceiveMail(User)

public Task< ReceiveMailResult > **ReceiveMailAsync**(MailType **mailType**, string **mailInDate**);


## 파라미터

| Value      | Type     | Description                 |
| ---------- | -------- | --------------------------- |
| mailType   | MailType | 전체 수령할 우편의 종류     |
| mailInDate | string   | 아이템을 받은 우편의 inDate |

### MailType

| Value    | Description                                            |
| -------- | ------------------------------------------------------ |
| Admin    | 콘솔에서 발송하는 관리자 우편                          |
| Rank     | 랭킹 결산 후 자동으로 지급되는 랭킹 우편               |
| Coupon   | 쿠폰 전용 웹에서 쿠폰 코드를 입력하여 받는 우편        |
| **User** | **유저끼리 자신의 데이터를 이용하여 발송한 유저 우편** |

## 설명

우편의 indate를 이용하여 우편 한 개를 수령합니다.  
**관리자 우편, 쿠폰 우편, 랭킹 우편의 경우 수령 시 리턴값이 동일합니다.**  
유저 우편은 별도의 리턴값이 제공됩니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- indate가 존재하더라도 MailType이 다를 경우 수령할 수 없습니다.  
- 수령 후에 우편이 **자동으로 삭제되지 않으며**, DeleteUserMail 함수를 호출할 경우에만 삭제됩니다.  
- 만료일이 지난 우편은 수령할 수 없습니다.  
- **수령 후 자동으로 유저의 DB에 아이템의 정보가 삽입/수정되지 않으며, 수동으로 클라이언트에서 아이템의 정보를 수령인의 게임 정보에 업데이트하는 과정이 필요합니다.**  

## Example

### Task 방식

```js
var mailType = MailType.User;

//우편 리스트 불러오기
var reqResult = await BackndMail.Instance.GetMailsAsync(mailType, 10);
//우편 리스트중 0번째 우편의 inDate 가져오기
var recentPostIndate = reqResult.AsUserResult().GetInfo(0).InDate;

// 동일한 PostType의 우편 수령하기
var receiveResult = await BackndMail.Instance.ReceiveMailAsync(mailType, recentPostIndate);
```

### Callback 방식
```js
//우편의 종류
var mailType = MailType.User;

//우편 리스트 불러오기
BackndMail.Instance.GetMails(mailType, 10, callback =>
{
     //우편 리스트중 0번째 우편의 inDate 가져오기
    var recentPostIndate = reqResult.AsUserResult().GetInfo(0).InDate;

     // 우편 수령하기
     BackndMail.Instance.ReceiveMail(mailType, recentPostIndate, callback2 =>
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

var createResult = await BackndUserData.Instance.CreateDataAsync(tableName, param);
var rowIndate = createResult.GetInDate();
var postItem = new MailType
{
    Title = "우편 제목",
    Content = "우편 내용",
    TableName = tableName,
    RowInDate = rowInDate,
    Column = columnName
};
var sendResult = await BackndMail.Instance.SendUserMailAsync(userIndate, postItem);
```

### Sample Code

```js
public async Task ReceivePostOneItemTest()
{
    var reqResult = await BackndMail.Instance.GetMailsAsync(MailType.User, 100);
    if (reqResult.IsSuccess() == false)
    {
        Debug.Log("우편을 불러오는 중 에러가 발생했습니다.");
        return;
    }

    //우편 리스트중 0번째 우편의 inDate 가져오기
    var mainInDate = reqResult.AsUserResult().GetInfo(0).InDate;
    var receiveResult = await BackndMail.Instance.ReceiveMailAsync(MailType.User, mainInDate);
    if (receiveResult.IsSuccess() == false)
    {
        Debug.LogError("우편 모두 수령하기 중 에러가 발생하였습니다. : " + receiveResult);
        return;
    }

    var infoList = receiveResult.GetRows("postItems");
    for (int i = 0; i < infoList.Count; i++)
    {
        var info = infoList[i];

        // 이 후, 전달받은 타입에 맞게 Json에서 parsing하여 사용.
    }
}
```
