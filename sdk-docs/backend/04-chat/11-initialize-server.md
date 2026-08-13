---
sidebar_label: "채팅 서버 접속 초기화"
draft: "true"
unlisted: "true"
description: "ResetConnect"
---

# ResetConnect

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public void **ResetConnect**();

## 설명

채팅 서버와의 소켓 연결을 강제로 초기화합니다.  
ResetConnect 함수를 호출하면 공용 채널, 길드 채널 접속을 모두 해제합니다.  
접속 해제 후 `OnLeaveChannel`과 `OnLeaveGuildChannel` 이벤트가 호출됩니다.  

## Example

```js
Backend.Chat.ResetConnect();
```
