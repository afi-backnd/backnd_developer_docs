---
sidebar_label: "귓속말 메시지 송신"
draft: "true"
unlisted: "true"
description: "Whisper"
---

# Whisper

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public void **Whisper**(string **ToNickname**, string **Message**);

## 파라미터

| Value      | Type   | Description                     |
| :--------- | :----- | :------------------------------ |
| ToNickname | string | 귓속말을 수신한 사람의 nickname |
| Message    | string | 귓속말 메시지 내용              |

## 설명

귓속말을 메시지를 송신합니다.  
귓속말은 일반 채널(Public Channel)에 접속하여 있는 경우에만 송/수신 가능합니다.  
귓속말 메시지의 최대 길이는 256byte입니다.  
다른 일반 채팅 채널에 접속한 유저에게도 귓속말을 송신할 수 있습니다.  

## Example

```js
Backend.Chat.Whisper("닉네임", "정원에서 만나~");
```
