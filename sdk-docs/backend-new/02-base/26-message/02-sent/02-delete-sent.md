---
sidebar_label: 보낸 쪽지 삭제
description: "보낸 쪽지 삭제"
---

# DeleteSentMessage

public Task< RequestResult > **DeleteSentMessageAsync**(string **messageIndate**);

## 파라미터

| Value         | Type   | Description                        |
| ------------- | ------ | ---------------------------------- |
| messageIndate | string | 삭제하고자 하는 보낸 쪽지의 indate |

## 설명

보낸 쪽지 리스트에 있는 쪽지를 삭제합니다.  
보낸 쪽지를 삭제하더라도 받은 사람의 받은 쪽지 목록은 남아있습니다.  

## Example

### Task 방식

```js
//삭제하고자 하는 쪽지의 inDate를 얻습니다.  
var reqResult = await BackndMessage.Instance.GetSentMessagesAsync();
string messageIndate = reqResult.GetInfo(0).InDate;

var reqResult = await BackndMessage.Instance.DeleteSentMessageAsync(messageIndate);
```

### Callback 방식

```js
//삭제하고자 하는 쪽지의 inDate를 얻습니다.  
BackndMessage.Instance.GetSentMessages(callback =>
{
      string messageIndate = reqResult.GetInfo(0).InDate;

      BackndMessage.Instance.DeleteSentMessage(messageIndate, callback2 =>
      {
        //이후 처리
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
