---
sidebar_label: "[Deprecated] 채팅 메시지 수신 이벤트"
---

# OnMatchChat

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝매치에서 제공되는 채팅 기능은 뒤끝챗을 기반으로 하고있으며 25년 2월 28일까지만 이용 가능합니다.  
:::

public MatchChatEventHandler **OnMatchChat**;

## 전달인자

| Value | Type               | Description |
| :---- | :----------------- | :---------- |
| args  | MatchChatEventArgs | 채팅 정보   |

## MatchChatEventArgs

| Value        | Type              | Description                                                                  |
| :----------- | :---------------- | :--------------------------------------------------------------------------- |
| ErrInfo      | ErrorCode         | 에러 정보                                                                    |
| Reason       | string            | 에러 이유                                                                    |
| From         | SessionInfo       | 채팅 메시지를 보낸 세션 정보                                                 |
| ChatModeType | MatchChatModeType | 채팅 메시지 타입 MatchChatModeType.Team(팀) 혹은 MatchChatModeType.All(전체) |
| Message      | string            | 채팅 메시지                                                                  |

## 설명

서버에서 클라이언트가 보낸 채팅 메시지를 브로드캐스팅 했을 때 호출되는 이벤트 핸들러입니다.  
자기 자신이 메시지를 보냈을 때도 호출됩니다.  

팀 메시지의 경우에도 전체 클라이언트에게 브로드캐스팅됩니다.  
클라이언트에서 팀 메시지인지, 전체 메시지인지 필터링을 수행해야 합니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnMatchChat = (MatchChatEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 누군가 전체 메시지를 보낸 경우 **  
ErrInfo : ErrorCode.Success  
Session : "채팅 메시지를 보낸 세션"  
ChatModeType : MatchChatModeType.All  
Message : "보낸 메시지"

** 누군가 팀 메시지를 보낸 경우 **  
ErrInfo : ErrorCode.Success  
Session : "채팅 메시지를 보낸 세션"  
ChatModeType : MatchChatModeType.Team  
Message : "보낸 메시지"  
