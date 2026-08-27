---
sidebar_label: 채팅 메시지
description: "채팅 메시지"
---

# 채팅 메시지

여러 채널에 동시에 입장을 한 상태라도 같은 콜백이 호출됩니다.  
메시지에 있는 채널 정보를 토대로 메시지를 따로 출력하는 로직을 만들어야 합니다.

## 설명

채팅 메시지에 관련된 콜백 및 호출 함수, 정보 클래스입니다.

### 콜백 함수

```csharp
// 채팅 메시지가 오는 콜백 함수 입니다. 메시지 정보를 넘겨 줍니다.
public void OnChatMessage(MessageInfo messageInfo) { }

// 귓속말 메시지가 오는 콜백 함수 입니다. 메시지 정보를 넘겨 줍니다.
public void OnWhisperMessage(WhisperMessageInfo messageInfo) { }

// 콘솔에서 메시지를 숨길 경우 오는 콜백 함수 입니다. 메시지 정보를 넘겨 줍니다.
public void OnHideMessage(MessageInfo messageInfo) { }

// 콘솔에서 메시지를 삭제 할 경우 오는 콜백 함수 입니다. 메시지 정보를 넘겨 줍니다.
public void OnDeleteMessage(MessageInfo messageInfo) { }
```

### 호출 함수

```csharp
// 채팅 메시지를 입력 하는 함수 입니다. 채널 그룹, 채널 이름, 채널 번호, 메시지를 넣어서 보내 줍니다.
ChatClient.SendChatMessage(string channelGroup, string channelName, UInt64 channelNumber, string message)

// 귓속말을 입력 하는 함수입니다. 유저 이름(귓속말 보낼), 메시지를 넣어서 보내 줍니다.
ChatClient.SendWhisperMessage(string gamerName, string message)
```

### 정보 클래스

#### MESSAGE_TYPE (enum)

| Value          | Description      |
| :------------- | :--------------- |
| SYSTEM_MESSAGE | 관리자 메시지    |
| NORMAL_MESSAGE | 일반 채팅 메시지 |

```csharp
public class MessageInfo
{
  // 채널 그룹 이름
  public string ChannelGroup = string.Empty;

  // 채널 이름
  public string ChannelName = string.Empty;

  // 채널 번호
  public UInt64 ChannelNumber = 0;

  // 유저 이름
  public string GamerName = string.Empty;

  // 아바타 이름
  public string Avatar = string.Empty;

  // 메시지를 작성한 유저의 언어 정보
  public string Language = string.Empty;

  // 메시지를 작성한 유저의 메타데이터
  public Dictionary<string, string> Metadata = new Dictionary<string, string>();

  // 채팅 인덱스
  public UInt64 Index = 0;

  // 채팅 메시지 타입
  public MESSAGE_TYPE MessageType = MESSAGE_TYPE.NORMAL_MESSAGE;

  // 채팅 메시지
  public string Message = string.Empty;

  // 채팅 시간 정보(UTC 기준) yyyy-MM-dd HH:mm:ss  ex) 2020-01-01 00:00:00
  public string Time = string.Empty;

  // 메시지 태그
  public string Tag = string.Empty;
}
```

```csharp
public class WhisperMessageInfo
{
  // 귓속말 인덱스
  public UInt64 Index = 0;

  // 귓속말 보낸 유저 이름
  public string FromGamerName = string.Empty;

  // 귓속말 보낸 유저 아바타 이름
  public string FromAvatar = string.Empty;

  // 귓속말을 보낸 유저의 언어 정보
  public string FromLanguage = string.Empty;

  // 귓속말을 보낸 유저의 메타데이터
  public Dictionary<string, string> FromMetadata = new Dictionary<string, string>();

  // 귓속말 메시지
  public string Message = string.Empty;

  // 귓속말 시간 정보(UTC 기준) yyyy-MM-dd HH:mm:ss  ex) 2020-01-01 00:00:00
  public string Time = string.Empty;

  // 메시지 태그
  public string Tag = string.Empty;
}
```
