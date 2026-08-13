---
sidebar_label: "[Deprecated] 채팅 메시지 송신"
description: "ChatToGameRoom"
---

# ChatToGameRoom

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝매치에서 제공되는 채팅 기능은 뒤끝챗을 기반으로 하고있으며 25년 2월 28일까지만 이용 가능합니다.  
:::

public void **ChatToGameRoom**(MatchChatModeType **matchChatModeType**, string **message**);

## 파라미터

| Value             | Type              | Description                                                                             |
| :---------------- | :---------------- | :-------------------------------------------------------------------------------------- |
| matchChatModeType | MatchChatModeType | 전체 메시지, 팀 메시지 여부 MatchChatModeType.Team(팀) 혹은 MatchChatModeType.All(전체) |
| message           | string            | 전송할 문자열                                                                           |

## 설명

채팅 메시지를 서버로 전송합니다.  
서버에 송신된 메시지는 서버에서 모든 클라이언트에게 브로드캐스팅합니다.  

> 모든 클라이언트에서는 데이터를 송신한 클라이언트를 포함합니다.  

## Example

```js
Backend.Match.ChatToGameRoom(matchChatModeType, message);
```
