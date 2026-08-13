---
sidebar_label: "길드 채널 접속 이벤트"
draft: "true"
unlisted: "true"
description: "OnJoinGuildChannel"
---

# OnJoinGuildChannel

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public JoinChannelEventHandler **OnJoinGuildChannel**;

## 전달인자

| Value   | Type        | Description                          |
| :------ | :---------- | :----------------------------------- |
| ErrInfo | ErrorInfo   | 성공/실패 정보                       |
| Session | SessionInfo | 채널에서 입장한 게이머의 SessionInfo |

## args 타입 명

| Value | Type                 | Description                   |
| :---- | :------------------- | :---------------------------- |
| args  | JoinChannelEventArgs | 채팅 서버 접속 성공/실패 여부 |

## 설명

유저가 채팅 채널에 입장할 때마다 호출되는 이벤트입니다.  

- 자기 자신 혹은 다른 유저가 채널에 입장한 경우 호출됩니다.  
- 자기 자신이 채널에 재접속 한 경우 호출됩니다.  
- 채널에 30초동안 유저가 존재하지 않을 경우(0명일 경우) 해당 채널은 자동으로 삭제되며, 다시 GetGroupChannelList()를 호출하여 새로운 채널을 불러와야합니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/chat/pingpong)가 호출되어야 합니다.  

:::info 데이터 통신 환경의 IP 할당 문제 안내
**데이터 통신환경의 IP 할당 문제**로 인해 데이터 통신 환경(3G/LTE/5G)에서 간헐적으로 채팅 서버 접속이 이루어지지 않는 현상이 발생될 수 있습니다.  

해당 현상은 뒤끝챗 뿐 아니라 은행 앱 등 서버와 통신이 필요한 다양한 종류의 어플리케이션에서 동일하게 발생하는 상황으로  
아래 안내드리는 방법을 통해 스마트폰에 부여된 IP주소를 갱신하여 해결이 가능하니 확인하여 채팅 서버 접속 불가 현상을 문의하는 유저분들께 안내해 주세요.  

**채팅 서버 접속 불가 상황**  
JoinChannel 호출 시 자동으로 `OnJoinGuildChannel()`함수가 호출되며 채널에 접속되어야 하지만 `OnJoinGuildChannel()`함수가 호출되지 않고 반응이 없는 상태가 지속되는 상황
	
**대처 방안**
* **비행기 모드를 20~30초간 유지했다가 끄고 이용**
* **스마트폰 네트워크 재설정 후 이용**
  * 아이폰 : 설정 - 일반 - 재설정 - 네트워크 재설정
  * 안드로이드 : 설정 - 일반 - 초기화 - 네트워크 설정 초기화
* 단, 네트워크 재설정 진행 시 와이파이, 블루투스 등 모든 네트워크 설정이 초기화 됩니다.  
:::

## Example

```js
//첫 번째 방법
Backend.Chat.OnJoinGuildChannel = (JoinChannelEventArgs args) =>
{
    Debug.Log(string.Format("OnJoinGuildChannel {0}", args.ErrInfo));
    //입장에 성공한 경우
    if(args.ErrInfo == ErrorInfo.Success)
    {
        // 내가 접속한 경우
        if(!args.Session.IsRemote)
        {
            Debug.Log("채널에 접속했습니다");
        }
        //다른 유저가 접속한 경우
        else
        {
            Debug.Log(args.Session.Nickname + "님이 접속했습니다");
        }
    }
    else
    {
        //에러가 발생했을 경우
        Debug.Log("입장 도중 에러가 발생했습니다 : " + args.ErrInfo.Reason));
    }
};

//두 번째 방법
Backend.Chat.OnJoinGuildChannel += (args) => {
  // 첫 번째 방법과 동일한 로직
}
```

## ArgumentCase

**채팅 서버 접속에 성공한 경우**  
args.ErrInfo: ErrorInfo.Success
ErrInfo.Category: Success  
ErrInfo.Detail  : Success  
ErrInfo.SocketError: Success

**채팅 서버 접속에 실패한 경우**  
ErrInfo.Category : ErrorCode.Exception  
ErrInfo.Reason : 접속 실패에 대한 이유

**가입하지 않은 길드의 길드 채널 접속을 시도한 경우(채팅 서버 접속 실패)**  
ErrInfo.Category: AuthenticationFailed  
ErrInfo.Detail  : DisconnectFromRemote  
ErrInfo.Reason: Incorrect guild.  

**채팅 서버에서 길드 정보 조회에 실패한 경우(채팅 서버 접속 실패)**  
ErrInfo.Category: AuthenticationFailed  
ErrInfo.Detail  : DisconnectFromRemote  
ErrInfo.Reason: fail to read guild info code: "code", reason: "reason"

**존재하지 않는 길드 채널에 접속을 시도한 경우(채팅 서버 접속 실패)**  
ErrInfo.Category: AuthenticationFailed  
ErrInfo.Detail  : DisconnectFromRemote  
ErrInfo.Reason: Invalid channel.  

**자동 접속 종료가 발생한 경우**  
ErrInfo.Category : ErrorCode.DisconnectFromRemote  
ErrInfo.Detail : ErrorCode.ChannelTimeout  
ErrInfo.Reason : [자신이 설정한 타임아웃 메시지](/sdk-docs/backend/chat/setting-auto-exit)
