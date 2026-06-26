---
sidebar_label: 뒤끝챗 활성화 여부 확인
draft: true
unlisted: true
---

# GetChatStatus

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public BackendReturnObject **GetChatStatus**();

## 설명

뒤끝 콘솔에서 뒤끝챗을 활성화했는지 확인합니다.  

> 채팅은 뒤끝 콘솔 > 뒤끝챗에서 활성화할 수 있습니다.  

## Example

### 동기

```js
Backend.Chat.GetChatStatus();
```

### 비동기

```js
Backend.Chat.GetChatStatus((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Chat.GetChatStatus, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**서버에 요청을 정상적으로 보낸 경우**  
statusCode : 200  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

```js
{
    "chatServerStatus":
    {
        "chatServer":"y" // y: 활성 상태, n: 비활성 상태
    }
}
```
