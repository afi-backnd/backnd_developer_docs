---
sidebar_label: 일반 채널 접속 종료 이벤트
draft: true
unlisted: true
---

# OnLeaveChannel

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public LeaveChannelEventHandler **OnLeaveChannel**;

## 전달인자

| Value | Type                  | Description              |
| :---- | :-------------------- | :----------------------- |
| args  | LeaveChannelEventArgs | 채널 퇴장 성공/실패 여부 |

## LeaveChannelEventArgs

| Value   | Type        | Description                          |
| :------ | :---------- | :----------------------------------- |
| ErrInfo | ErrorInfo   | 성공/실패 정보                       |
| Session | SessionInfo | 채널에서 퇴장한 게이머의 SessionInfo |

## 설명

유저가 채팅 채널에서 접속이 종료된 경우 호출됩니다.  

- 자신 혹은 다른 유저가 명시적으로 LeaveChannel 함수를 호출한 경우 호출됩니다.  
- 자신 혹은 다른 유저가 통신 환경 등이 좋지 않아 연결이 끊어진 후 재접속에 실패한 경우 호출됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/chat/pingpong)가 호출되어야 합니다.  

## Example

```js
//첫번째 방법
Backend.Chat.OnLeaveChannel= (LeaveChannelEventArgs args) =>
{
    Debug.Log(string.Format("OnLeaveChannel {0}", args.ErrInfo));
    //퇴장에 성공한 경우
    if(args.ErrInfo == ErrorInfo.Success)
    {
        // 내가 퇴장한 경우
        if(!args.Session.IsRemote)
        {
             Debug.Log("채널에서 퇴장했습니다");
        }
        // 다른 유저가 퇴장한 경우
        else
        {
            Debug.Log(args.Session.NickName + "님이 퇴장했습니다");
        }
    }
    else
    {
        // 에러가 발생했을 경우
        Debug.Log("퇴장 도중 에러가 발생했습니다 : " + args.ErrInfo.Reason);
    }
};

// 두번째 방법
Backend.Chat.OnLeaveChannel += (args) => {
  // 첫번째 방법과 같은 로직
}
```

## ArgumentCase

**자기자신이나 다른 유저가 채널에서 퇴장할 경우**  
ErrInfo.Category: Success  
ErrInfo.Detail  : Success  
ErrInfo.SocketError: Success

**1분 30초이상 클라이언트와 채팅 서버의 연결이 끊어졌을 경우(Poll함수 미사용등)**  
ErrInfo.Category: Exception  
ErrInfo.Detail  : DisconnectFromRemote  
ErrInfo.SocketError: Success  
ErrInfo.Reason: Disconnected from server.  

**ResetConnect()함수를 통해 로컬에서 강제로 접속을 끊었을 경우**  
ErrInfo.Category: Exception  
ErrInfo.Detail  : DisconnectFromLocal  
ErrInfo.SocketError: Success
