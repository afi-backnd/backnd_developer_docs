---
sidebar_label: 시작하기
description: "시작하기"
---

# 시작하기

Unity SDK를 통해 실시간 채팅 기능을 Unity 프로젝트에 빠르게 추가할 수 있어요.
이 가이드는 채팅 SDK를 설치하고, 첫 메시지를 보내고, 콘솔에서 메시지를 확인하기까지의 과정을 단계별로 안내합니다.

:::tip 가장 빠르게 채팅 기능을 테스트하고 싶다면?
뒤끝에서 제공하는 [예제 프로젝트](/sdk-docs/chat/example-game)를 활용해서 바로 시작해보세요!
이미 완성된 UI와 함께 실시간 채팅이 작동하는 과정을 직접 확인하고, 필요한 부분만 가져다 쓸 수 있습니다.
:::

### 1. 채팅 SDK 설치하기

아래 패키지를 다운로드하고 유니티 프로젝트에 Import 합니다.
- <a href="https://developer.thebackend.io/sdk/chat/1.4.0/BackndChat-1.4.0.unitypackage" id="download-sdk-chat">BackndChat-1.4.0.unitypackage</a>&nbsp;[2025-12-18]

:::info 베이스 SDK와 함께 사용한다면?
채팅 SDK를 임포트할 때 `Backend.dll`이 덮어씌워지지 않도록 체크 해제하세요. 자세한 내용은 [베이스 SDK와 함께 사용하는 경우](/sdk-docs/chat/install/with-base-sdk) 문서에서 확인하세요.
:::

:::caution 안드로이드 빌드 시 AGP 버전 8.5 이하일 경우

`mainTemplate.gradle` 파일에 아래의 내용을 추가해야 합니다.

```
android {
  ...
  packagingOptions {
      jniLibs {
        useLegacyPackaging true
      }
  }
}
``` 

:::

### 2. 인증 키 설정하기

:::info 인증 키 설정을 하는 이유는?
채팅 서비스는 기본적으로 베이스의 유저 기능을 사용합니다. 따라서 채팅 SDK를 사용하려면 뒤끝 콘솔에서 발급받은 인증 키를 Unity 프로젝트에 설정해야 합니다.
만일 베이스의 유저 기능을 사용하지 않고 싶으시다면 [커스텀 인증](/sdk-docs/chat/custom-auth)를 참고하세요.
:::

1. 뒤끝 콘솔의 채팅 > 설정 페이지에서 `Chat UUID`, `Client App ID`, `Signature Key` 를 복사합니다.
![chat settings](/img/docs/guide/chat/install/chat-inspector-keys.png)
2. Unity에 인증키를 붙여넣습니다.
![chat settings](/img/docs/guide/chat/install/chat-inspector-chatuuid.png)
![chat settings](/img/docs/guide/chat/install/chat-inspector-basekeys.png)
  - `The Backend > Edit Chat Settings`에서 Chat UUID 입력
  - `The Backend > Edit Settings`에서 Client App ID / Signature Key 입력

### 3. 유저 닉네임 설정하기

채팅을 사용하려면 유저의 닉네임을 반드시 설정해야 합니다.

```csharp
using UnityEngine;
using BackEnd;

public class BackendInit : MonoBehaviour
{
    void Start()
    {
        // 뒤끝 초기화
        Backend.Initialize();

        var result = Backend.BMember.CustomLogin("testId", "testPw");
        if (!result.IsSuccess())
        {
            Backend.BMember.CustomSignUp("testId", "testPw");
            // 닉네임 설정
            // highlight-next-line
            Backend.BMember.UpdateNickname("닉네임");
        }
    }
}
```

### 4. 채팅 클라이언트 초기화

채팅 메시지를 주고받기 위해 `ChatClient`를 초기화합니다.

```csharp
using System.Collections.Generic;
using UnityEngine;
using BackndChat;

public class UIChatExample : MonoBehaviour, BackndChat.IChatClientListener
{
    private ChatClient ChatClient;

    void Start()
    {
        ChatClient = new ChatClient(this, new ChatClientArguments
        {
            Avatar = "default",
            Metadata = new Dictionary<string, string>()
            {
                { "Rank", "1" },
                { "Level", "1000" },
                { "Server", "1" }
            }
        });
    }

    void Update() => ChatClient?.Update();
    void OnApplicationQuit() => ChatClient?.Dispose();

    public void OnChatMessage(MessageInfo messageInfo)
    {
        Debug.Log($"[수신] {messageInfo.GamerName}: {messageInfo.Message}");
    }

    public void OnSuccess(SUCCESS_MESSAGE success, object param) { }
    public void OnError(ERROR_MESSAGE error, object param) { }
}
```

:::note
뒤끝 채팅 SDK는 네트워크 변경이나 일시적인 연결 끊김 상황에서도 자동으로 다시 연결되는 기능을 제공합니다. `ChatClient`를 초기화한 뒤, 종료 시점에 `Dispose()`만 호출하면 별도의 재연결 처리를 따로 구현하지 않아도 됩니다.
:::

### 5. 메시지 보내기

채널에 메시지를 전송해봅니다.
뒤끝 채팅을 활성화하면 자동으로 `global` 그룹의 `server-1` 이라는 채널이 생성되므로, 해당 채널에 메시지를 보내봅시다.

```csharp {10,11,14,15,16,17,18}
// 채널에 입장하고 메시지를 보내는 흐름

void Start()
{
    ChatClient = new ChatClient(this, new ChatClientArguments
    {
        // ...
    });

    // 기본 채널 입장 (콘솔에서 자동 생성된 채널)
    ChatClient.SendJoinOpenChannel("global", "server-1");
}

public void OnJoinChannel(ChannelInfo channelInfo)
{
    // 채널 입장 완료 시 메시지 전송
    ChatClient.SendChatMessage(channelInfo.ChannelGroup, channelInfo.ChannelName, channelInfo.ChannelNumber, "안녕하세요!");
}
```


<details>
<summary>완전한 코드 템플릿</summary>
<div markdown="1">

### 뒤끝 초기화 스크립트
    
```csharp
using BackEnd;
using UnityEngine;

public class LoginManager : MonoBehaviour
{
    public InputField Username = null;

    public InputField Password = null;

    void Start()
    {
        Backend.Initialize(true);
    }

    void Login()
    {
        var returnObject = Backend.BMember.CustomLogin(Username.text, Password.text);
        if (false == returnObject.IsSuccess())
        {
            Debug.Log("CustomLogin Fail");

            returnObject = Backend.BMember.CustomSignUp(Username.text, Password.text);
            if (false == returnObject.IsSuccess())
            {
                Debug.Log("CustomSignUp Fail");
                return;
            }

            returnObject = Backend.BMember.CustomLogin(Username.text, Password.text);
            if (false == returnObject.IsSuccess())
            {
                Debug.Log("CustomLogin Fail");
                return;
            }

            // 닉네임 업데이트 - 닉네임이 없을 경우 채팅 접속이 불가능 합니다.
            Backend.BMember.UpdateNickname(Username.text);
        }
    }
}
```

### 채팅 초기화 스크립트

```csharp
public class UIChatManager : MonoBehaviour, BackndChat.IChatClientListener
{
    private BackndChat.ChatClient ChatClient = null;

    void Start()
    {
        ChatClient = new ChatClient(this, new ChatClientArguments
        {
            Avatar = "default",
            Metadata = new Dictionary<string, string>() 
            {
                { "Rank", "1" },
                { "Level", "1000" },
                { "Server", "Server 1" },
            },
        });
        
        // 인스펙터를 설정하지 않았을 경우, 아래와 같이 Chat UUID (xxxx-xxxx-xxxxxx-xxxxxxx)를 등록할 수 있습니다.
        /*
            ChatClient = new ChatClient(this, new ChatClientArguments
            {
                UUID = "xxxx-xxxx-xxxxxx-xxxxxxx",
                Avatar = "default",
                Metadata = new Dictionary<string, string>() 
                {
                    { "Rank", "1" },
                    { "Level", "1000" },
                    { "Server", "Server 1" },
                },
            });
        */

        // 커스텀 인증 서버를 사용 하실 경우, 아래와 같이 토큰을 등록할 수 있습니다.
        /*
            ChatClient = new ChatClient(this, new ChatClientArguments
            {
                UUID = "xxxx-xxxx-xxxxxx-xxxxxxx",
                Avatar = "default",
                Metadata = new Dictionary<string, string>() 
                {
                    { "Rank", "1" },
                    { "Level", "1000" },
                    { "Server", "Server 1" },
                },
                CustomAccessToken = USER_TOKEN,
            });
        */
    }

    void Update()
    {
        ChatClient?.Update();
    }

    public void OnJoinChannel(ChannelInfo channelInfo) { }

    public void OnLeaveChannel(ChannelInfo channelInfo) { }

    public void OnJoinChannelPlayer(string channelGroup, string channelName, UInt64 channelNumber, PlayerInfo player) { }

    public void OnLeaveChannelPlayer(string channelGroup, string channelName, UInt64 channelNumber, PlayerInfo player) { }

    public void OnUpdatePlayerInfo(string channelGroup, string channelName, ulong channelNumber, PlayerInfo player) { }

    public void OnChangeGamerName(string oldGamerName, string newGamerName) { }

    public void OnChatMessage(MessageInfo messageInfo) { }

    public void OnWhisperMessage(WhisperMessageInfo messageInfo) { }

    public void OnTranslateMessage(List<MessageInfo> messages) { }

    public void OnHideMessage(MessageInfo messageInfo) { }

    public void OnDeleteMessage(MessageInfo messageInfo) { }

    public void OnSuccess(SUCCESS_MESSAGE success, object param)
    {
        switch(success)
        {
            default:
                break;
        }
    }

    public void OnError(ERROR_MESSAGE error, object param)
    {
        switch(error)
        {
            default:
                break;
        }
    }

    private void OnApplicationQuit()
    {
        ChatClient?.Dispose();
    }
}
```

</div>
</details>


### 6. 콘솔에서 메시지 확인하기

뒤끝 콘솔에서 `server-1` 채널을 선택하면, 클라이언트에서 보낸 채팅 메시지를 확인할 수 있습니다.
![first message](/img/docs/guide/chat/install/first-message.png)

## 채팅 연동이 완료되었습니다! 🎉

이제 실시간 채팅 기능을 게임에서 사용할 수 있게 되었습니다.  
더 확장하거나 실제 프로젝트에 적용하려면 아래 문서들을 참고하세요.

- **GUI까지 완성된 샘플을 보고 싶어요** → [예제 프로젝트](/sdk-docs/chat/example-game)
- **다른 채팅 기능이 궁금해요** → [채팅 기능 소개](/guide/getting-started/how-to-use/backnd-chat)
- **뒤끝 베이스랑 함께 쓰고 싶어요** → [베이스 연동 가이드](/sdk-docs/chat/install/with-base-sdk)
- **자체 인증 시스템을 쓰고 싶어요** → [커스텀 인증 가이드](/sdk-docs/chat/custom-auth)
