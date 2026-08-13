---
sidebar_label: "도배 방지 문구 설정"
draft: "true"
unlisted: "true"
description: "SetRepeatedChatBlockMessage"
---

# SetRepeatedChatBlockMessage

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public void **SetRepeatedChatBlockMessage**(out ErrorInfo **errorInfo**);

## 파라미터

| Value   | Type   | Description    |                         default                         |
| :------ | :----- | :------------- | :-----------------------------------------------------: |
| message | string | 성공/실패 정보 | 도배 방지를 위해 일정 시간 동안 채팅 입력이 금지됩니다. |

## 설명

콘솔에서 설정된 일정 시간 이내에, 일정 횟수 이상 입력할 경우, 일정 시간 동안 채팅 입력이 금지되며, 채팅 입력 시 각각의 핸들러에서 에러가 리턴됩니다.  
도배 방지 에러 발생 시, 리턴될 메시지를 설정합니다.  

- 횟수는 길드/공용 채널 합산입니다.  
- 한쪽 채널에서 입력 금지당하면 모든 채널에서 입력 금지를 당합니다.  

설정된 메시지는 아래 핸들러에 args의 ErrorInfo에서 확인할 수 있습니다.  

```js
Backend.Chat.OnChat = (ChatEventArgs args) => {}
Backend.Chat.OnGuildChat = (ChatEventArgs args) => {}
Backend.Chat.OnWhisper = (WhisperEventArgs args) => {}
```

## Example

```js
Backend.Chat.SetRepeatedChatBlockMessage("도배하면 안돼요.");
```
