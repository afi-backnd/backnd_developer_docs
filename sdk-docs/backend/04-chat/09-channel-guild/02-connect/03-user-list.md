---
sidebar_label: "유저 목록 이벤트"
draft: "true"
unlisted: "true"
description: "OnSessionListInGuildChannel"
---

# OnSessionListInGuildChannel

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public SessionListInChannelHandler **OnSessionListInGuildChannel**;

## 전달인자

| Value | Type                          | Description                                   |
| :---- | :---------------------------- | :-------------------------------------------- |
| args  | SessionListInChannelEventArgs | 채널 입장 시, 채널에 존재하는 유저들의 리스트 |

## SessionListInChannelEventArgs

| Value       | Type      | Description                                      |
| :---------- | :-------- | :----------------------------------------------- |
| ErrInfo     | ErrorInfo | 성공/실패 정보                                   |
| SessionList | List      | 채널에 접속하고 있는 모든 게이머들의 정보 리스트 |

## 설명

현재 채널에 접속해 있는 모든 유저의 정보를 조회합니다.  
자기 자신이 채팅 채널 입장에 성공했을 때 최초 한 번만 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/chat/pingpong)가 호출되어야 합니다.  

## Example

```js
// 첫 번째 방법
Backend.Chat.OnSessionListInGuildChannel = (SessionListInChannelEventArgs  args) =>
{
    Debug.Log(string.Format("OnSessionListInGuildChannel {0}", args.ErrInfo));
    List<string> nameList = new List<string>();

    // 성공한 경우
    if(args.ErrInfo == ErrorInfo.Success)
    {
        // 게이머 정보를 참여자 리스트에 추가
        foreach(SessionInfo session in args.SessionList)
        {
            Debug.Log(session.NickName);
            nameList.Add(session.NickName);
        }
    }
    else
    {
        // 실패한 경우
        Debug.Log("유저 목록 불러오기 중 에러가 발생했습니다 : " + args.ErrInfo.Reason);
    }
};

// 두 번째 방법
Backend.Chat.OnSessionListInGuildChannel += (args) => {
  // 첫 번째 방법과 동일한 로직
}
```

## ArgumentCase

**유저 정보 불러오기에 성공한 경우**  
ErrInfo.Category: Success  
ErrInfo.Detail  : Success  
ErrInfo.SocketError: Success
