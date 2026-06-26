---
sidebar_label: 유저 우편 보내기
description: "유저 우편 보내기"
---

# SendUserMail

public Task< RequestResult > **SendUserMailAsync**(string **receiverIndate**, MailItemParams **itemParam**)

:::info 안내
기존 우편 기능의 유저 우편 보내기(Social.Post.SendPost)와 동일한 기능입니다.  
:::

## 파라미터

| Value          | Type     | Description          |
| :------------- | :------- | :------------------- |
| receiverIndate | string   | 수령할 유저의 indate |
| itemParam       | MailItemParams | 선물할 아이템의 정보 |

### MailItemParams

| Value     | Type   | Description                                                                             |
| :-------- | :----- | :-------------------------------------------------------------------------------------- |
| Title     | string | 우편 제목. 1-20자 제한, 영문 대/소문자, 숫자, 한글, 특수문자, 띄어쓰기 가능.            |
| Content   | string | (Optional) 우편 내용. 1-50자 제한, 영문 대/소문자, 숫자, 한글, 특수문자, 띄어쓰기 가능. |
| TableName | string | 게임 아이템의 게임 정보 테이블                                                          |
| RowInDate | string | 게임 정보 테이블의 row key                                                              |
| Column    | string | 게임 정보 테이블의 column key                                                           |

## 설명

게임 정보관리에 있는 정보를 다른 게임 유저에게 우편으로 보냅니다.  

- 한 유저당 하루 최대 50건의 우편 발송이 가능합니다.  
- 이미 발송된 우편은 취소(회수)가 불가능합니다.  
- 우편 전송 후, 서버에서 해당 셀(cell)의 게임 정보 삭제가 이루어집니다. 해당 셀의 정보는 **"NULL":true**으로 수정됩니다.  

### 우편 보내기 프로세스

게임 정보를 바탕으로 우편 발송이 이루어지기 때문에, 우편 발송 전 필수로 게임 정보를 최신 상태로 업데이트해야 합니다.  
우편 발송 이후, 해당 유저의 게임 정보는 삭제되고("NULL":true), 받는 게임 유저의 우편 리스트에 들어가게 됩니다.  
따라서 발송 이후에 다시 유저의 게임 정보를 가져와 클라이언트 내의 게임 정보를 다시 업데이트하는 과정이 필요합니다.(우편을 받는 사람의 게임정 보가 자동으로 업데이트되지 않습니다)

1. 게임 정보 업데이트
  (서버의 저장된 유저의 데이터를 최신 상태로 업데이트. 이미 최신 상태라는 것이 보장되면 업데이트를 하지 않아도 됩니다.)

2. 우편 발송
  (클라이언트에서 우편을 발송하면 서버에서는 보낸 사람의 row에서 해당 column의 데이터를 삭제하고("NULL":true로 바꾸고) 받는 사람에게 우편 발송합니다.)

3. 보낸 사람의 row에서 해당 column 데이터가 삭제된 내역("NULL":true로 바뀐 내역)을 클라이언트에서 갱신
  (우편 발송 이후 게임 정보 조회 함수를 이용하여 유저의 현재 정보를 조회하여 클라이언트를 갱신해야 합니다.)

## Example

### Task 방식

```js
MailItemParams itemParam = new MailItemParams
{
    Title = "이거 너 가져~",
    Content = "난 필요 없어ㅋㅋ",
    TableName = table_name,
    RowInDate = indate,
    Column = column_name
};
var reqResult = await BackndMail.Instance.SendUserMailAsync("ReceiverIndate", itemParam);
```

### Callback 방식

```js
MailItemParams itemParam = new MailItemParams
{
    Title = "이거 너 가져~",
    Content = "난 필요 없어ㅋㅋ",
    TableName = table_name,
    RowInDate = indate,
    Column = column_name
};
BackndMail.Instance.SendUserMail("ReceiverIndate", postItem, (callback) =>
{
    // 이후 처리
}) ;
```


## ReturnCase

### Success cases

**보내기에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**title, contents 가 최소 길이/최대 길이를 만족하지 않는 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad {title/content} length, 잘못된 {title/content} length 입니다

**자기 자신에게 보낸 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden don't send yourself, 금지된 don't send yourselfS

**indate(게임 정보)가 잘못된 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : it doesn't exist not found, it doesn't exist을(를) 찾을 수 없습니다

**하루에 50개 이상의 우편을 보낸 경우**  
statusCode : 429  
errorCode : Too Many Request  
message : 하루에 50개만 가능합니다. 요청 횟수를 초과하였습니다.  
