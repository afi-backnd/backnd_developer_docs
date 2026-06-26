---
sidebar_label: 유저 우편 삭제하기
description: "유저 우편 삭제하기"
---

# DeleteUserMail

public Task< RequestResult > **DeleteUserMailAsync**(string **mailIndate**)

:::info 안내
기존 우편 기능의 유저 우편 삭제(Social.Post.DeleteUserPostItem)와 동일한 기능입니다.  
:::

## 파라미터

| Value      | Type   | Description               |
| ---------- | ------ | ------------------------- |
| postIndate | string | 삭제할 user post의 indate |

## 설명

우편 리스트에 있는 유저 우편을 삭제합니다.  
관리자 우편은 삭제할 수 없습니다.  

삭제 시, 수령하지 않은 아이템이 있다면 함께 삭제됩니다.  

## Example

### Task 방식

```js
// 삭제할 유저 우편의 inDate를 얻습니다.  
var getResult = await BackndMail.Instance.GetMailsAsync(MailType.User);
var postIndate = getResult.AsUserResult().GetInfoList()[0].InDate;

var deleteResult = await BackndMail.Instance.DeleteUserMailAsync(postIndate);
```

### Callback 방식

```js
// 삭제할 유저 우편의 inDate를 얻습니다.  
BackndMail.Instance.GetMails(PostType.User, callback =>
{
   var postIndate = getResult.AsUserResult().GetInfoList()[0].InDate;

   BackndMail.Instance.DeleteUserMail(postIndate, callback2 =>
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

**존재하지 않는 postIndate를 입력한 경우**  
statusCode : 400  
errorCode : ConditionalCheckFailedException  
message : The conditional request faile
