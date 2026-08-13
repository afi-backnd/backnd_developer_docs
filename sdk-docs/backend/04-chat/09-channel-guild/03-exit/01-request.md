---
sidebar_label: "길드 채널 접속 종료 요청"
draft: "true"
unlisted: "true"
description: "LeaveChannel"
---

# LeaveChannel

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public void **LeaveChannel**(ChannelType **type**);

## 파라미터

| Value | Type               | Description                                                               |
| :---- | :----------------- | :------------------------------------------------------------------------ |
| type  | ChannelType(enum) | 퇴장할 채널의 종류. ChannelType.Public(일반) 혹은 ChannelType.Guild(길드) |


## 설명

현재 접속한 채널에서 퇴장을 시도합니다.  

## Example

```js
Backend.Chat.LeaveChannel(ChannelType.Guild);
```
