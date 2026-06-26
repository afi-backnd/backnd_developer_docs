---
sidebar_label: 귓속말 메시지 수신 이벤트
draft: true
unlisted: true
---

# OnWhisper

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::
---

# OnWhisper

public WhisperEventHandler **OnWhisper**;

## 전달인자

| Value | Type             | Description                           |
| :---- | :--------------- | :------------------------------------ |
| args  | WhisperEventArgs | 다른 유저로부터 온 귓속말 메시지 정보 |

## WhisperEventArgs

| Value   | Type        | Description                 |
| :------ | :---------- | :-------------------------- |
| ErrInfo | ErrorInfo   | 성공/실패 정보              |
| From    | SessionInfo | 귓속말을 송신한 사람의 정보 |
| To      | SessionInfo | 귓속말을 수신한 사람의 정보 |
| Message | string      | 귓속말 메시지 내용          |

## 설명

자신에게 온 귓속말을 수신합니다.  

- 일반 채팅 채널에 접속했을 때만 귓속말 메시지를 수신할 수 있습니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/chat/pingpong)가 호출되어야 합니다.  

## Example

```js
// 첫 번째 방법
Backend.Chat.OnWhisper = (WhisperEventArgs args) =>
{
    Debug.Log(string.Format("OnWhisper {0}", args.ErrInfo));

    if(args.ErrInfo == ErrorInfo.Success)
    {
        Debug.Log(string.Format("OnWhisper: from {0} to {1} : message {2}", args.From.NickName, args.To.NickName, args.Message));

        // 내가 보낸 귓속말인 경우
        if(!args.From.IsRemote)
        {
            Debug.Log("나 : " + args.Message);
        }
        // 내가 받은 귓속말인 경우
        else
        {
            Debug.Log(string.Format("{0}님 : {1}", args.From.NickName, args.Message));
        }
    }
    else if(args.ErrInfo.Category == ErrorCode.BannedChat)
    {
        // 도배방지 메시지
        if(args.ErrInfo.Detail == ErrorCode.BannedChat)
        {
           Debug.Log("메시지를 너무 많이 입력하였습니다. 일정 시간 후에 다시 시도해 주세요");
        }
    }
};

// 두 번째 방법
Backend.Chat.OnWhisper += (args) => {
  // 첫 번째 방법과 같은 로직
}
```
