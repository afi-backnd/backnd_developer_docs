---
sidebar_label: "관리자 공지 메시지 수신 이벤트"
draft: "true"
unlisted: "true"
description: "OnNotification"
---

# OnNotification

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public NotificationEventHandler **OnNotification**;

## 전달인자

| Value | Type                  | Description                                                  |
| :---- | :-------------------- | :----------------------------------------------------------- |
| args  | NotificationEventArgs | `뒤끝 콘솔 > 뒤끝챗 > 공지 관리`에서 보낸 메시지에 대한 정보 |

## NotificationEventArgs

| Value   | Type   | Description                           |
| :------ | :----- | :------------------------------------ |
| Subject | string | 콘솔에서 보낸 공지 메시지의 발송명    |
| Message | string | 콘솔에서 보낸 공지 메시지의 발송 내용 |

## 설명

`뒤끝 콘솔 > 뒤끝챗 > 공지 관리 > 공지 발송`을 통해 발송된 공지 메시지를 콜백하는 핸들러입니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/chat/pingpong)가 호출되어야 합니다.  

## Example

```js
// 첫 번째 방법
Backend.Chat.OnNotification = (NotificationEventArgs args) =>
{
    Debug.Log(string.Format("제목 : {0} / 내용 : {1}", args.Subject, args.Message));
};

// 두 번째 방법
Backend.Chat.OnNotification += (args) => {
  // 첫 번째 방법과 동일한 로직
}
```
