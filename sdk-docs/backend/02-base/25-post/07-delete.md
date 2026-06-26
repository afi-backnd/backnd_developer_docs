---
sidebar_label: 유저 우편 삭제하기
---

# DeleteUserPost

public BackendReturnObject **DeleteUserPost**(string **postIndate**);

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

### 동기

```js
// 삭제할 유저 우편의 inDate를 얻습니다.  
var bro = Backend.UPost.GetPostList(PostType.User);

string postIndate = bro.GetReturnValuetoJSON()["postList"][0]["inDate"].ToString();

Backend.UPost.DeleteUserPost(postIndate);
```

### 비동기

```js
// 삭제할 유저 우편의 inDate를 얻습니다.  
Backend.UPost.GetPostList(PostType.User, callback =>
{
   string postIndate = callback.GetReturnValuetoJSON()["postList"][0]["inDate"].ToString();

   Backend.UPost.DeleteUserPost(postIndate, callback2 =>
   {
       // 이후 처리
   });

});
```

### SendQueue

```js
// 삭제할 유저 우편의 inDate를 얻습니다.  
SendQueue.Enqueue(Backend.UPost.GetPostList, PostType.User, callback =>
{
   string postIndate = callback.GetReturnValuetoJSON()["postList"][0]["inDate"].ToString();

   SendQueue.Enqueue(Backend.UPost.DeleteUserPost, postIndate, callback2 =>
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
