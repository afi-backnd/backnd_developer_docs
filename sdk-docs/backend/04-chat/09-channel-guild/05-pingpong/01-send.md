---
sidebar_label: "메시지 송신"
draft: "true"
unlisted: "true"
description: "ChatToChannel"
---

# ChatToChannel

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public bool **ChatToChannel**(ChannelType **type**, string **message**);

## 파라미터

| Value   | Type               | Description                                                                        |
| :------ | :----------------- | :--------------------------------------------------------------------------------- |
| type    | ChannelType(enum) | 메시지를 전송할 채널의 종류. ChannelType.Public(일반) 혹은 ChannelType.Guild(길드) |
| message | string             | 전송할 채팅 메시지                                                                 |

## 설명

현재 접속한 채널에 메시지를 보냅니다. 채팅 메시지의 최대 길이는 512byte입니다.  

## Example

```js
Backend.Chat.ChatToChannel(ChannelType.Guild, "안녕하세요. 반갑습니다.");
```

**송신을 시도한 채팅 메시지의 길이가 512byte를 초과한 경우**  
Maximum chat size is 512 byte

**채팅 채널에 접속하지 않았는데 메시지 송신을 시도한 경우**  
Not connected. (0)

**채팅 채널 접속이 완료되지 않았는데(OnJoinChannel 이벤트가 호출되지 않았는데) 메시지 송신을 시도한 경우**  
Not connected. (1)
