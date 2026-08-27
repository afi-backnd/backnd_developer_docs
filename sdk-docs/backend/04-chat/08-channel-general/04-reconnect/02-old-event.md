---
sidebar_label: "[Deprecated] 다른 유저 재접속 성공이벤트"
draft: "true"
unlisted: "true"
description: "[Deprecated] OnSessionOnlineChannel"
---

# \[Deprecated] OnSessionOnlineChannel

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public SessionOnlineEventHandler **OnSessionOnlineChannel**;

:::caution 지원 종료 안내
* 해당 기능은 5.5.0 버전부터 지원이 중단되었으며, 일시적으로 채팅 서버와 끊어졌을 경우 완전히 연결을 끊고 <a href  = "/sdk-docs/backend/chat/channel-general/deconnect/event">OnLeaveChannel</a>핸들러가 호출됩니다.  
:::

## 전달인자

| Value | Type                   | Description                     |
| :---- | :--------------------- | :------------------------------ |
| args  | SessionOnlineEventArgs | 채팅 서버 재접속 성공/실패 여부 |

## SessionOnlineEventArgs

| Value   | Type        | Description                             |
| :------ | :---------- | :-------------------------------------- |
| ErrInfo | ErrorInfo   | 성공/실패 정보                          |
| Session | SessionInfo | 채널과 접속이 끊긴 게이머의 SessionInfo |

## 설명

다른 유저가 일반 채널과 연결이 일시적으로 끊어진 후 다시 연결되었을 때 호출되는 이벤트입니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/chat/pingpong)가 호출되어야 합니다.  

## Example

```js
// 첫 번째 방법
Backend.Chat.OnSessionOnlineChannel = (SessionOnlineEventArgs args) =>
{
    Debug.Log(string.Format("OnSessionOnlineChannel {0}", args.ErrInfo));
    // 재입장에 성공한 경우
    if(args.ErrInfo == ErrorInfo.Success)
    {
        // 내가 접속한 경우
        if(!args.Session.IsRemote)
        {
            Debug.Log("채널에 재접속했습니다");
        }
        // 다른 유저가 접속한 경우
        else
        {
            Debug.Log(args.Session.Nickname + "님이 재접속했습니다");
        }
    }
    else
    {
        // 에러가 발생했을 경우
        Debug.Log("재입장도중 에러가 발생했습니다 : " + args.ErrInfo.Reason));
    }
};

// 두 번째 방법
Backend.Chat.OnSessionOnlineChannel += (args) => {
  // 첫 번째 방법과 동일한 로직
}
```

## ArgumentCase

**해당 유저가 채팅 서버 재접속에 성공한 경우**  
ErrInfo.Category: Success  
ErrInfo.Detail  : NetworkOnline  
ErrInfo.SocketError: Success  
ErrInfo.Reason: Session Reconnect
