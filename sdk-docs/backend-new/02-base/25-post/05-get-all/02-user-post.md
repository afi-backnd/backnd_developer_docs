---
sidebar_label: 유저 우편 모두 수령
---

# ReceiveAllMails(User)

public Task< ReceiveMailResult > **ReceiveAllMailsAsync**(MailType **mailType**);

## 파라미터

| Value    | Type     | Description             |
| -------- | -------- | ----------------------- |
| mailType | MailType | 전체 수령할 우편의 종류 |

### MailType

| Value    | Description                                            |
| -------- | ------------------------------------------------------ |
| Admin    | 콘솔에서 발송하는 관리자 우편                          |
| Rank     | 랭킹 결산 후 자동으로 지급되는 랭킹 우편               |
| **User** | **유저끼리 자신의 데이터를 이용하여 발송한 유저 우편** |

## 설명

현재까지 조회된 모든 우편들을 수령합니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- 수령할 우편이 존재하지 않을 경우 에러가 발생합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndMail.Instance.ReceiveAllMailsAsync(MailType.User);

// 모두 수령에 대한 로직은 'Sample Code'를 참고해주세요
```

### Callback 방식

```js
BackndMail.Instance.ReceiveAllMails(PostType.User, (callback) =>
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

## Sample Code

```js
public async Task ReceivePostItemAllTest()
{
    var reqResult = await BackndMail.Instance.GetMailsAsync(MailType.User, 100);
    if (reqResult.IsSuccess() == false)
    {
        Debug.Log("우편을 불러오는 중 에러가 발생했습니다.");
        return;
    }

    var receiveResult = await BackndMail.Instance.ReceiveAllMailsAsync(MailType.User);
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
