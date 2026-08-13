---
sidebar_label: "최근채팅내역 이벤트"
draft: "true"
unlisted: "true"
description: "OnRecentChatLogs"
---

# OnRecentChatLogs

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public RecentChatLogsEventHandler **OnRecentChatLogs**;

## 전달인자

| Value | Type                    | Description                |
| :---- | :---------------------- | :------------------------- |
| args  | RecentChatLogsEventArgs | 최근 채팅 내역에 대한 정보 |

## RecentChatLogsEventArgs

| Value       | Type                       | Description                                                             |
| :---------- | :------------------------- | :---------------------------------------------------------------------- |
| ErrInfo     | ErrorInfo                  | 성공/실패 정보                                                          |
| channelType | ChannelType(enum)          | 해당 채널의 종류. ChannelType.Public(일반) 혹은 ChannelType.Guild(길드) |
| LogInfos    | List< RecentChatLogsInfo > | 최근에 보낸 메시지 내역                                                 |

## RecentChatLogsInfo

| Value    | Type   | Description                   |
| :------- | :----- | :---------------------------- |
| NickName | string | 메시지를 보냈던 유저의 닉네임 |
| Message  | string | 보냈던 메시지의 내용          |

## 설명

최근 채팅 내역 불러오기 비동기 함수와는 **별개의 이벤트**로, 채팅 채널 입장 시 호출되는 이벤트입니다.  
**일반/길드 채널 입장 시**에 호출됩니다.  

- **일반/길드 채널의 내역**만 불러오며, 귓속말이나 공지/전체 채팅은 포함되지 않습니다.  
- 메시지 내역은 **최대 30개**까지 불러올 수 있습니다.  
- 채널이 0명인 상태로 30초 이상이 지난 경우 해당 채널에 대한 로그 기록은 지워지며, 그 이전의 로그를 불러올 수 없습니다.(해당 부분은 **최근 채팅 내역 불러오기 비동기 함수**를 통해 보완할 수 있습니다.)

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/chat/pingpong)가 호출되어야 합니다.  

## Example

```js
// 첫 번째 방법
Backend.Chat.OnRecentChatLogs = (RecentChatLogsEventArgs args) => {

  Debug.Log(args.ErrInfo.ToString());
  Debug.Log("들어간 채널 종류 : " + args.channelType.ToString());

  for(int i=0;i < args.LogInfos.Count; i++)
  {
      Debug.Log("닉네임 : " + args.LogInfos[i].NickName);
      Debug.Log("메시지 : " + args.LogInfos[i].Message);
  }
};

//두 번째 방법
Backend.Chat.OnRecentChatLogs += (args) => {
 // 첫 번째 방법과 동일한 로직
}
```
