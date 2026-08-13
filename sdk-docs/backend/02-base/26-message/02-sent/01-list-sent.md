---
sidebar_label: "보낸 쪽지 조회"
description: "GetSentMessageList"
---

# GetSentMessageList

public BackendReturnObject **GetSentMessageList**();

## 설명

유저들에게 보낸 쪽지 리스트를 조회합니다.  
해당 리스트는 받은 쪽지의 리스트(GetReceivedMessageList)와 공통된 값을 공유합니다.(쪽지 받은 사람/보낸 사람의 정보로 구별)

> **[콘솔 - 프로젝트 설정 - 소셜 설정]**에서 설정한 최대 보유수만큼 리스트를 보유할 수 있으며, 리스트가 가득 찬 상태에서 쪽지를 보내면 가장 오래된 쪽지가 삭제되고 최신 쪽지가 리스트에 추가됩니다.  

## Example

### 동기

```js
Backend.Message.GetSentMessageList();
```

### 비동기

```js
Backend.Message.GetSentMessageList((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Message.GetSentMessageList, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

받은 쪽지 리스트를 조회하는 함수(GetReceivedMessageList)와 공통된 값을 공유하며 receiver과 sender를 통해 구분해야 합니다.  

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
public void GetSentMessageListTest()
{
    var bro = Backend.Message.GetSentMessageList();

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData json = bro.FlattenRows();

    List<MessageItem> messageList = new List<MessageItem>();

    for(int i = 0; i < json.Count; i++)
    {
        MessageItem messageItem = new MessageItem();

        messageItem.receiver = json[i]["receiver"].ToString();
        messageItem.sender = json[i]["sender"].ToString();
        messageItem.content = json[i]["content"].ToString();
        messageItem.inDate = json[i]["inDate"].ToString();
        messageItem.senderNickname = json[i]["senderNickname"].ToString();
        messageItem.isRead = json[i]["isRead"].ToString() == "true" ? true : false;
        messageItem.receiverNickname = json[i]["receiverNickname"].ToString();
        messageItem.isReceiverDelete = json[i]["isReceiverDelete"].ToString() == "true" ? true : false;
        messageItem.isSenderDelete = json[i]["isSenderDelete"].ToString() == "true" ? true : false;

        messageList.Add(messageItem);
        Debug.Log(messageItem.ToString());
    }
}
```

---

# GetSentMessage

public BackendReturnObject **GetSentMessage**(string **messageIndate**);

## 파라미터

| Value         | Type   | Description                    |
| ------------- | ------ | ------------------------------ |
| messageIndate | string | 읽고자 하는 보낸 쪽지의 indate |

## 설명

보낸 쪽지 한 개의 정보를 조회합니다.  

## Example

### 동기

```js
// 읽고자 하는 쪽지의 inDate를 얻습니다.  
BackendReturnObject bro = Backend.Message.GetSentMessageList();
string messageIndate = bro.Rows()[0]["inDate"]["S"].ToString();

//읽고자 하는 쪽지의 내용을 읽어옵니다.  
BackendReturnObject bro2 = Backend.Message.GetSentMessage(messageIndate);
string content = bro2.GetReturnValuetoJSON()["row"]["content"]["S"].ToString();
```

### 비동기

```js
// 읽고자 하는 쪽지의 inDate를 얻습니다.  
Backend.Message.GetSentMessageList(callback =>
{
      string messageIndate = callback.Rows()[0]["inDate"]["S"].ToString();

      //읽고자 하는 쪽지의 내용을 읽어옵니다.  
      Backend.Message.GetSentMessage(messageIndate, callback2 =>
      {
        string content = callback2.GetReturnValuetoJSON()["row"]["content"]["S"].ToString();
      });

});
```

### SendQueue

```js
// 읽고자 하는 쪽지의 inDate를 얻습니다.  
SendQueue.Enqueue(Backend.Message.GetSentMessageList, callback =>
{
      string messageIndate = callback.Rows()[0]["inDate"]["S"].ToString();

      //읽고자 하는 쪽지의 내용을 읽어옵니다.  
      SendQueue.Enqueue(Backend.Message.GetSentMessage, messageIndate, callback2 =>
      {
          string content = callback2.GetReturnValuetoJSON()["row"]["content"]["S"].ToString();
      });

});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

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
public void GetSentMessageTest()
{
    var bro = Backend.Message.GetSentMessage("2022-03-14T07:21:01.953Z");

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData json = bro.GetFlattenJSON();

    MessageItem messageItem = new MessageItem();

    messageItem.receiver = json["row"]["receiver"].ToString();
    messageItem.sender = json["row"]["sender"].ToString();
    messageItem.content = json["row"]["content"].ToString();
    messageItem.inDate = json["row"]["inDate"].ToString();
    messageItem.senderNickname = json["row"]["senderNickname"].ToString();
    messageItem.isRead = json["row"]["isRead"].ToString() == "true" ? true : false;
    messageItem.receiverNickname = json["row"]["receiverNickname"].ToString();
    messageItem.isReceiverDelete = json["row"]["isReceiverDelete"].ToString() == "true" ? true : false;
    messageItem.isSenderDelete = json["row"]["isSenderDelete"].ToString() == "true" ? true : false;

    Debug.Log(messageItem.ToString());
}
```
