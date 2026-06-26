---
sidebar_label: 유저 우편 불러오기
---

# GetMails(MailType.User)

public Task< GetMailsResult > **GetMailsAsync**(MailType**User** **mailType**);  
public Task< GetMailsResult > **GetMailsAsync**(MailType**User** **mailType**, int **limit**);

## 파라미터

| Value    | Type     | Description                                                                     |
| -------- | -------- | ------------------------------------------------------------------------------- |
| mailType | MailType | 불러올 우편의 종류                                                              |
| limit    | int      | 불러올 우편의 갯수. 최소 10개, 최대 100개. 미입력 시 limit은 10개로 지정됩니다. |

> limit이 10 미만일 경우, 10으로 고정됩니다.  

### MailType

| Value    | Description                                                                    |
| -------- | ------------------------------------------------------------------------------ |
| Admin    | 콘솔에서 발송하는 관리자 우편                                                  |
| Rank     | 랭킹 결산 후 자동으로 지급되는 랭킹 우편                                       |
| Coupon   | 뒤끝 콘솔의 웹 쿠폰 설정에서 생성한 페이지로 쿠폰을 사용 후 발송되는 쿠폰 우편 |
| **User** | **유저끼리 자신의 데이터를 이용하여 발송한 유저 우편**                         |

## 설명

우편 종류(MailType)에 따른 우편 리스트를 불러옵니다.  

각 MailType의 종류마다 리턴값에 추가된 값이 존재하거나 없을 수 있으므로 아래 ReturnValueJson을 참고해주시기 바랍니다.  

> 유저 우편의 경우, 관리자 우편과 랭킹 우편과는 Json 타입이 크게 다를 수 있으므로 주의해주시기 바랍니다.  

- 리턴값에 기존 우편 기능(Backend.Social.Post) 리턴값에 존재했던 데이터 타입 "S", "L", "M"이 존재하지 않습니다.  
- 유저 우편의 경우, 아이템 수령 시 아이템이 삭제되지 않으며 해당 아이템을 다시 수령할 수 없습니다.  
- 유저 우편을 삭제하고 싶을 경우 유저 우편 삭제(BackndMail.Instance.DeleteUserMail) 함수를 이용해 주세요.  
- 유저 우편의 수령 여부는 receivedDate 키의 존재 여부로 확인할 수 있습니다.  
- 유저 우편의 만료일은 발송일 기준 15일이며, 만료된 유저 우편은 다시 발신인에게 돌아가지 않습니다.  

## Example

### Task 방식

```js
var reqResult = await BackndMail.Instance.GetMailsAsync(MailType.User, 10);
var infoList = reqResult.AsUserResult().GetInfoList();

for (int i = 0; i < infoList.Count; i++)
{
    Debug.Log("제목 : " + infoList[i].Title);
    Debug.Log("inDate : " + infoList[i].InDate);
}
```

### Callback 방식

```js
BackndMail.Instance.GetMails(MailType.User, 10, callback =>
{
    var infoList = reqResult.AsUserResult().GetInfoList();

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
returnValue : ReturnValueJson을 참조

**불러올 우편이 없을 경우**  
statusCode : 200  
message : Success  
returnValue : { "postList" : []}

## ReturnValueJson

### 유저 우편

```js
{
    "postList": [
        {
            "content": "우편 내용", // 내용
            "expirationDate": "2022-01-04T10:58:34.360Z", // 만료 날짜
            "receiverInDate": "2021-11-09T02:16:35.406Z", // 우편이 등록된 날짜(보낸 날짜)
            "item": { // 보낸 아이템 정보
                "key1": "item1",
                "key2": "item2",
                "key3": "item3"
            },
            "itemLocation": { // 해당 아이템이 위치해있던 테이블 정보
                "column": "dic", // 컬럼
                "table": "tableName" // 테이블
            },
            "receiverNickname": "유저2", // 받는 유저의 이름
            "receivedDate":"2021-12-28T00:23:35.484Z" // 수령한 날짜(수령했을 경우에만 보임)
            "sender": "0fdd6e40-4103-11ec-b4f7-4b3fc8e50bcf", // 보내는 유저의 uuid
            "inDate": "2021-12-20T10:58:34.360Z", // 우편의 inDate
            "senderNickname": "유저3", // 보낸 유저의 닉네임
            "senderInDate": "2021-11-09T02:16:34.852Z", // 보낸 유저의 inDate
            "sentDate": "2021-12-20T10:58:34.360Z", // 보낸 날짜
            "title": "우편 제목" // 우편의 제목
        },
        {
            "content": "우편 내용",
            "expirationDate": "2022-01-04T10:58:33.918Z",
            "receiverInDate": "2021-11-09T02:16:35.406Z",
            "item": {
                "key1": "아이템1",
                "key2": "아이템2",
                "key3": "아이템3"
            },
            "itemLocation": {
                "column": "dic",
                "table": "tableName"
            },
            "receiverNickname": "유저2",
            "sender": "0fdd6e40-4103-11ec-b4f7-4b3fc8e50bcf",
            "inDate": "2021-12-20T10:58:33.918Z",
            "senderNickname": "유저1",
            "senderInDate": "2020-01-09T02:12:34.822Z",
            "sentDate": "2021-12-20T10:58:33.918Z",
            "title": "우편 제목"
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
var mailParam = new MailItemParams
{
    Title = "우편 제목",
    Content = "우편 내용",
    TableName = tableName,
    RowInDate = rowIndate,
    Column = columnName
};
var sendResult = await BackndMail.Instance.SendUserMailAsync(userIndate, mailParam);
```

## Sample Code

해당 예제는 관리자 우편, 랭킹 우편, 쿠폰 우편에서 공통적으로 사용가능합니다.  

```js
public async Task GetPostListTest()
{
    int limit = 100;
    var postType = MailType.User;
    var getResult = await BackndMail.Instance.GetMailsAsync(postType, limit);
    if (!getResult.IsSuccess())
    {
        Debug.LogError(getResult.ToString());
        return;
    }

    var postList = getResult.GetRows("postList");
    foreach (var postToken in postList)
    {
        var postJson = postToken as JObject;

        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.AppendLine("title : " + postJson["title"].ToString());
        stringBuilder.AppendLine("inDate : " + postJson["inDate"].ToString());

        stringBuilder.AppendLine("sentDate : " + postJson["sentDate"].ToString());
        stringBuilder.AppendLine("expirationDate : " + postJson["expirationDate"].ToString());
        stringBuilder.AppendLine("receiverInDate : " + postJson["receiverInDate"].ToString());
        stringBuilder.AppendLine("sender : " + postJson["sender"].ToString());
        stringBuilder.AppendLine("senderInDate : " + postJson["senderInDate"].ToString());

        if (postJson.ContainsKey("receiverNickname"))
        {
            stringBuilder.AppendLine("receiverNickname : " + postJson["receiverNickname"].ToString());
        }

        if (postJson.ContainsKey("senderNickname"))
        {
            stringBuilder.AppendLine("senderNickname : " + postJson["senderNickname"].ToString());
        }

        if (postJson.ContainsKey("receivedDate"))
        {
            stringBuilder.AppendLine("receivedDate : " + postJson["receivedDate"].ToString());
        }

        stringBuilder.AppendLine("item의 테이블 : " + postJson["itemLocation"]["table"].ToString());
        stringBuilder.AppendLine("item의 컬럼 : " + postJson["itemLocation"]["column"].ToString());

        stringBuilder.AppendLine("item : ");

        var items = postJson["item"] as JObject;
        foreach (var property in items.Properties())
        {
            stringBuilder.AppendLine($"| {property.Name} : {property.Value.ToString()}");
        }

        Debug.Log(stringBuilder.ToString());
    }
}
```
