---
sidebar_label: 보낸 쪽지 조회
---

# GetSentMessages

public Task< GetMessagesResult > **GetSentMessagesAsync**();

## 설명

유저들에게 보낸 쪽지 리스트를 조회합니다.  
해당 리스트는 받은 쪽지의 리스트(GetReceivedMessages)와 공통된 값을 공유합니다.(쪽지 받은 사람/보낸 사람의 정보로 구별)

> **[콘솔 - 프로젝트 설정 - 소셜 설정]**에서 설정한 최대 보유수만큼 리스트를 보유할 수 있으며, 리스트가 가득 찬 상태에서 쪽지를 보내면 가장 오래된 쪽지가 삭제되고 최신 쪽지가 리스트에 추가됩니다.  

## Example

### Task 방식

```js
var reqResult = await BackndMessage.Instance.GetSentMessagesAsync();
```

### Callback 방식

```js
BackndMessage.Instance.GetSentMessages((callback) =>
{
  // 이후 처리
});
```


## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

## ReturnValueJson

받은 쪽지 리스트를 조회하는 함수(GetReceivedMessages)와 공통된 값을 공유하며 receiver과 sender를 통해 구분해야 합니다.  

```js
{
  rows: [
    {
      // 쪽지 받은 사람의 inDate
      receiver: { S: "2018-12-18T01:58:18.722Z" },
      // 쪽지 보낸 사람의 inDate
      sender: { S: "2018-12-27T00:21:31.322Z" },
      // 쪽지 내용
      content: { S: "hello this is user7. How are you doing?" },
      // 쪽지의 inDate
      inDate: { S: "2018-12-27T00:26:22.083Z" },
      // 쪽지 보낸 사람의 닉네임
      senderNickname: { S: "user7" },
      // 받은 사람이 읽었는지 판단하는 기준
      isRead: { BOOL: false },
      // 쪽지 받은 사람의 닉네임
      receiverNickname: { S: "ryan" },
      // 쪽지 받은 사람의 쪽지 삭제 여부
      isReceiverDelete: { BOOL: false },
      // 쪽지 보낸 사람의 쪽지 삭제 여부
      isSenderDelete: { BOOL: false },
    },
    {
      receiver: [Object],
      sender: [Object],
      content: [Object],
      inDate: [Object],
      senderNickname: [Object],
      isRead: [Object],
      receiverNickname: [Object],
      isReceiverDelete: [Object],
      isSenderDelete: [Object],
    },
  ];
}
```

## Sample Code

```js
public class MessageItem
{
    public string receiver;
    public string sender;
    public string content;
    public string inDate;
    public string senderNickname;
    public bool isRead;
    public string receiverNickname;
    public bool isReceiverDelete;
    public bool isSenderDelete;

    public override string ToString()
    {
        return $"receiver : {receiver}\n" +
        $"sender : {sender}\n" +
        $"content : {content}\n" +
        $"inDate : {inDate}\n" +
        $"senderNickname : {senderNickname}\n" +
        $"isRead : {isRead}\n" +
        $"receiverNickname : {receiverNickname}\n" +
        $"isReceiverDelete : {isReceiverDelete}\n" +
        $"isSenderDelete : {isSenderDelete}\n";
    }
};
```

```js
public async Task GetSentMessageListTest()
{
    var reqResult = await BackndMessage.Instance.GetSentMessagesAsync();
    if (!reqResult.IsSuccess())
        return;

    var infoList = reqResult.GetInfoList();
    List<MessageItem> messageList = new List<MessageItem>();

    for (int i = 0; i < infoList.Count; i++)
    {
        var info = infoList[i];            

        MessageItem messageItem = new MessageItem();
        messageItem.receiver = info.Receiver;
        messageItem.sender = info.Sender;
        messageItem.content = info.Content;
        messageItem.inDate = info.InDate;
        messageItem.senderNickname = info.SenderNickname;
        messageItem.isRead = info.IsRead;
        messageItem.receiverNickname = info.ReceiverNickname;
        messageItem.isReceiverDelete = info.IsReceiverDelete;
        messageItem.isSenderDelete = info.IsSenderDelete;

        messageList.Add(messageItem);
        Debug.Log(messageItem.ToString());
    }
}
```

---

# GetSentMessageByInDate

public Task< GetSingleMessageResult > **GetSentMessageByInDateAsync**(string **messageIndate**);

## 파라미터

| Value         | Type   | Description                    |
| ------------- | ------ | ------------------------------ |
| messageIndate | string | 읽고자 하는 보낸 쪽지의 indate |

## 설명

보낸 쪽지 한 개의 정보를 조회합니다.  

## Example

### Task 방식

```js
// 읽고자 하는 쪽지의 inDate를 얻습니다.  
var reqResult = await BackndMessage.Instance.GetSentMessagesAsync();
string messageIndate = reqResult.GetInfo(0).InDate;

//읽고자 하는 쪽지의 내용을 읽어옵니다.  
var reqResult2 = await BackndMessage.Instance.GetSentMessageByInDateAsync(messageIndate);
string content = reqResult2.GetInfo().Content;
```

### Callback 방식

```js
// 읽고자 하는 쪽지의 inDate를 얻습니다.  
BackndMessage.Instance.GetSentMessages(callback =>
{
      string messageIndate = callback.GetInfo(0).InDate;

      //읽고자 하는 쪽지의 내용을 읽어옵니다.  
      BackndMessage.Instance.GetSentMessageByInDate(messageIndate, callback2 =>
      {
        string content = callback2.GetInfo().Content;
      });
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

## ReturnValueJson

```js
{
        row:
        {
            receiver: // 쪽지 받은 사람의 inDate
                { S : "2018-12-18T01:58:18.722Z" },
            sender: // 쪽지 보낸 사람의 inDate
                { S : "2018-12-27T00:21:31.322Z" },
            content: // 쪽지 내용
                { S : "hello this is user7. How are you doing?" },
            inDate: // 쪽지의 inDate
                { S : "2018-12-27T00:26:22.083Z" },
            senderNickname: // 쪽지 보낸 사람의 닉네임
                { S : "user7" },
            isRead: // 받은 사람이 읽었는지 판단하는 기준
                { BOOL : false },
            receiverNickname: // 쪽지 받은 사람의 닉네임
                { S : "ryan" },
            isReceiverDelete: // 쪽지 받은 사람의 쪽지 삭제 여부
                { BOOL : false },
            isSenderDelete:  // 쪽지 보낸 사람의 쪽지 삭제 여부
                { BOOL : false }
        }
}
```

## Sample Code

```js
public class MessageItem
{
    public string receiver;
    public string sender;
    public string content;
    public string inDate;
    public string senderNickname;
    public bool isRead;
    public string receiverNickname;
    public bool isReceiverDelete;
    public bool isSenderDelete;

    public override string ToString()
    {
        return $"receiver : {receiver}\n" +
        $"sender : {sender}\n" +
        $"content : {content}\n" +
        $"inDate : {inDate}\n" +
        $"senderNickname : {senderNickname}\n" +
        $"isRead : {isRead}\n" +
        $"receiverNickname : {receiverNickname}\n" +
        $"isReceiverDelete : {isReceiverDelete}\n" +
        $"isSenderDelete : {isSenderDelete}\n";
    }
};
```

```js
public async Task GetSentMessageTest()
{
    var reqResult = await BackndMessage.Instance.GetSentMessageByInDateAsync("2022-03-14T07:21:01.953Z");
    if (!reqResult.IsSuccess())
        return;

    var info = reqResult.GetInfo();

    MessageItem messageItem = new MessageItem();
    messageItem.receiver = info.Receiver;
    messageItem.sender = info.Sender;
    messageItem.content = info.Content;
    messageItem.inDate = info.InDate;
    messageItem.senderNickname = info.SenderNickname;
    messageItem.isRead = info.IsRead;
    messageItem.receiverNickname = info.ReceiverNickname;
    messageItem.isReceiverDelete = info.IsReceiverDelete;
    messageItem.isSenderDelete = info.IsSenderDelete;

    Debug.Log(messageItem.ToString());
}
```
