---
sidebar_label: "유저 신고"
draft: "true"
unlisted: "true"
description: "ReportUser"
---

# ReportUser

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public BackendReturnObject **ReportUser**(string **ReportedNickname**, string **Details**);  
public BackendReturnObject **ReportUser**(string **ReportedNickname**, string **Reason**, string **Details**);

## 파라미터

| Value            | Type   | Description                 |
| :--------------- | :----- | :-------------------------- |
| ReportedNickname | string | 신고 당하는 유저의 닉네임   |
| Reason           | string | (Optional) 신고 사유        |
| Details          | string | 신고 내용(최대길이: 500자) |

## 설명

특정 유저를 관리자에게 신고합니다. 콘솔에서 신고 내용을 확인할 수 있습니다.  

## Example

### 동기

```js
Backend.Chat.ReportUser("ReportedNickname", "Reasons", "Details");
```

### 비동기

```js
Backend.Chat.ReportUser("ReportedNickname", "Reasons", "Details", (callback) => {
    // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Chat.ReportUser, "ReportedNickname", "Reasons", "Details", (callback) => {
    // 이후 처리
});
```

## ReturnCase

### Success cases

**신고에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : {"result":"id1님의 신고(id2님에 대한)가 접수 되었습니다."}

### Error cases

**details을 입력하지 않은 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad details of report, 잘못된 details of report 입니다

**details이 너무 긴 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad details, 잘못된 details 입니다

**신고 대상 닉네임의 유저가 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : 신고할 유저의 닉네임 not found, 신고할 유저의 닉네임을(를) 찾을 수 없습니다

