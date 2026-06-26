---
sidebar_label: 공지 메시지 수신 이벤트
draft: true
unlisted: true
---

# OnGlobalChat

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public GlobalChatEventHandler **OnGlobalChat**;

## 전달인자

| Value | Type                | Description                     |
| :---- | :------------------ | :------------------------------ |
| args  | GlobalChatEventArgs | 운영자로부터 전송된 메시지 정보 |

## GlobalChatEventArgs

| Value   | Type        | Description                      |
| :------ | :---------- | :------------------------------- |
| ErrInfo | ErrorInfo   | 성공/실패 정보                   |
| From    | SessionInfo | 공지 메시지를 송신한 사람의 정보 |
| Message | string      | 공지 메시지 내용                 |

## 설명

운영자가 보낸 메시지를 수신합니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/chat/pingpong)가 호출되어야 합니다.  

## Example

```js
// 첫 번째 방법
Backend.Chat.OnGlobalChat = (GlobalChatEventArgs args) =>
{
    Debug.Log(string.Format("OnGlobalChat {0}", args.ErrInfo));

    if(args.ErrInfo == ErrorInfo.Success)
    {
        Debug.Log(string.Format("[User Notification] {0} : ", args.From.NickName, args.Message));
    }
    else
    {
        Debug.Log(string.Format("[Error] {0}", "운영자가 아닙니다"));
    }
};

// 두 번째 방법
Backend.Chat.OnGlobalChat += (args) => {
  // 첫 번째 방법과 동일한 로직
}
```

## ArgumentCase

**운영자로부터 메시지가 올 경우**  
args.ErrInfo : ErrorInfo.Success
ErrInfo.Category: Success  
ErrInfo.Detail  : Success  
ErrInfo.SocketError: Success

**공지를 보냈는데, 운영자가 아닐 경우**  
ErrInfo.Category: InvalidOperation  
ErrInfo.Detail  : BannedChat  
ErrInfo.SocketError: Success  
ErrInfo.Reason: You are not admin.  
