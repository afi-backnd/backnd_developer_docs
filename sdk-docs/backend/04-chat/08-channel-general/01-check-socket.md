---
sidebar_label: 일반 채널 소켓접속여부 확인
draft: true
unlisted: true
---

# IsChatConnect

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::


public bool **IsChatConnect**(ChannelType **channelType**);

## 파라미터

| Value       | Type               | Description                                                               |
| :---------- | :----------------- | :------------------------------------------------------------------------ |
| channelType | ChannelType(enum) | 확인할 채널의 종류. ChannelType.Public(일반) 혹은 ChannelType.Guild(길드) |

## 설명

클라이언트가 채팅 서버 간에 소켓이 연결되어 있는지 확인합니다.  

- 해당 기능은 단순히 서버와 소켓이 연결되어 있는지 확인하는 함수로 채팅 서버에 요청을 보내 정상적으로 접속이 되어 있는지 확인하는 함수가 아닙니다.  
- 단순 소켓 접속 여부를 확인하는 함수입니다.  

## Example

```js
bool isConnect = Backend.Chat.IsChatConnect(ChannelType.Public);

    // 채팅 서버와 클라이언트가 연결되어 있을 경우
    // isConnect = true

    // 채팅 서버와 클라이언트가 연결되어 있지 않은 경우
    // isConnect = false

if(isConnect)
{
   Debug.Log("일반 채널에 연결되어 있습니다");
}
```

## Response Cases

| ReturnValue(bool) | Description                                      |
| :---------------- | :----------------------------------------------- |
| true              | 채팅 서버와 클라이언트가 연결되어 있는 경우      |
| false             | 채팅 서버와 클라이언트가 연결되어 있지 않은 경우 |
