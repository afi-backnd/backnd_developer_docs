---
sidebar_label: "받은 쪽지 삭제"
description: "DeleteReceivedMessage"
---

# DeleteReceivedMessage

public BackendReturnObject **DeleteReceivedMessage**(string **messageIndate**);

## 파라미터

| Value         | Type   | Description                        |
| ------------- | ------ | ---------------------------------- |
| messageIndate | string | 삭제하고자 하는 받은 쪽지의 indate |

## 설명

받은 쪽지 리스트에 있는 쪽지를 삭제합니다.  

## Example

### 동기

```js
//삭제하고자 하는 쪽지의 inDate를 얻습니다.  
BackendReturnObject bro = Backend.Message.GetReceivedMessageList();
string messageIndate = bro.Rows()[0]["inDate"]["S"].ToString();

Backend.Message.DeleteReceivedMessage(messageIndate);
```

### 비동기

```js
//삭제하고자 하는 쪽지의 inDate를 얻습니다.  
Backend.Message.GetReceivedMessageList(callback =>
{
      string messageIndate = callback.Rows()[0]["inDate"]["S"].ToString();

      Backend.Message.DeleteReceivedMessage(messageIndate, callback2 =>
      {
        //이후 처리
      });

});
```

### SendQueue

```js
//삭제하고자 하는 쪽지의 inDate를 얻습니다.  
SendQueue.Enqueue(Backend.Message.GetReceivedMessageList, callback =>
{
      string messageIndate = callback.Rows()[0]["inDate"]["S"].ToString();

      SendQueue.Enqueue(Backend.Message.DeleteReceivedMessage, messageIndate, callback2 =>
      {
          // 이후 처리
      });

});
```

## ReturnCase

### Success cases

**삭제에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**해당 messageIndate의 쪽지가 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : message not found, message을(를) 찾을 수 없습니다
