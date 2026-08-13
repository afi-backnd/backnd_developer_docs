---
sidebar_label: "자동 접속 종료 문구 설정"
draft: "true"
unlisted: "true"
description: "SetTimeoutMessage"
---

# SetTimeoutMessage

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public void **SetTimeoutMessage**(string **message**);

## 파라미터

| Value   | Type   | Description    |                    default                    |
| :------ | :----- | :------------- | :-------------------------------------------: |
| message | string | 성공/실패 정보 | 장시간 채팅 미입력으로 접속이 종료되었습니다. |

## 설명

콘솔에서 설정된 시간 동안 채팅 입력이 없을 시 채팅 서버 접속이 종료됩니다.  

해당 조건으로 접속 종료 시, 출력되는 메시지를 설정합니다.  

설정된 메시지는 아래 핸들러에 args의 ErrorInfo에서 확인할 수 있습니다.  

```js
Backend.Chat.OnLeaveChannel = (LeaveChannelEventArgs args) => {}
Backend.Chat.OnLeaveGuildChannel = (LeaveChannelEventArgs args) => {}
```

## Example

```js
Backend.Chat.SetTimeoutMessage("챗 안해서 쫒아냅니다.");
```
