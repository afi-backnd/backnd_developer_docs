---
sidebar_label: 받은 쪽지 조회
description: "받은 쪽지 조회"
---

# GetReceivedMessages

public Task< GetMessagesResult > **GetReceivedMessagesAsync**();

## 설명

받은 쪽지 리스트를 조회합니다.  
해당 리스트는 보낸 쪽지의 리스트(GetSentMessages)와 공통된 값을 공유합니다.(쪽지 받은 사람/보낸 사람의 정보로 구별)

> **[콘솔 - 프로젝트 설정 - 소셜 설정]**에서 설정한 최대 보유수만큼 리스트를 보유할 수 있으며, 쪽지가 가득 찬 경우 쪽지를 받을 수 없습니다.  

## Example

### Task 방식

```js
var reqResult = await BackndMessage.Instance.GetReceivedMessagesAsync();
```

### Callback 방식

```js
BackndMessage.Instance.GetReceivedMessages((callback) =>
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

보낸 쪽지 리스트를 조회하는 함수(GetSentMessages)와 공통된 값을 공유하며 receiver과 sender를 통해 구분해야 합니다.  

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
public async Task GetReceivedMessageListTest()
{
    var reqResult = await BackndMessage.Instance.GetReceivedMessagesAsync();
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

# GetReceivedMessageByInDate

public Task< GetSingleMessageResult > **GetReceivedMessageByInDateAsync**(string **messageIndate**);

## 파라미터

| Value         | Type   | Description                    |
| ------------- | ------ | ------------------------------ |
| messageIndate | string | 읽고자 하는 받은 쪽지의 indate |

## 설명

받은 쪽지 한 개를 읽습니다.  

## Example

### Task 방식

```js
// 읽고자 하는 쪽지의 inDate를 얻습니다.  
var reqResult = await BackndMessage.Instance.GetReceivedMessagesAsync();
string messageIndate = reqResult.GetInfo(0).InDate;

//읽고자 하는 쪽지의 내용을 읽어옵니다.  
var reqResult2 = await BackndMessage.Instance.GetReceivedMessageByInDateAsync(messageIndate);
string content = reqResult2.GetInfo().Content;
```

### Callback 방식

```js
// 읽고자 하는 쪽지의 inDate를 얻습니다.  
BackndMessage.Instance.GetReceivedMessages(callback =>
{
      string messageIndate = callback.GetInfo(0).InDate;

      //읽고자 하는 쪽지의 내용을 읽어옵니다.  
      BackndMessage.Instance.GetReceivedMessageByInDate(messageIndate, callback2 =>
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
public async Task GetReceivedMessageTest()
{
    var reqResult = await BackndMessage.Instance.GetReceivedMessageByInDateAsync("2022-03-14T07:21:01.953Z");
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
