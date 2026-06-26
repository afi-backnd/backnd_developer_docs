---
sidebar_label: 채팅 메시지
---

# 채팅 메시지

여러 채널에 동시에 입장을 한 상태라도 같은 콜백이 호출됩니다.  
메시지에 있는 채널 정보를 토대로 메시지를 따로 출력하는 로직을 만들어야 합니다.

## 설명

채팅 메시지에 관련된 콜백 및 호출 함수, 정보 클래스입니다.

### 콜백 함수

```cpp
// 채팅 메시지가 오는 콜백 함수 입니다. 메시지 정보를 넘겨 줍니다.
void OnChatMessage(BackndChat::MessageInfo messageInfo) { }

// 귓속말 메시지가 오는 콜백 함수 입니다. 메시지 정보를 넘겨 줍니다.
void OnWhisperMessage(BackndChat::WhisperMessageInfo messageInfo) { }

// 콘솔에서 메시지를 숨길 경우 오는 콜백 함수 입니다. 메시지 정보를 넘겨 줍니다.
void OnHideMessage(BackndChat::MessageInfo messageInfo) { }

// 콘솔에서 메시지를 삭제 할 경우 오는 콜백 함수 입니다. 메시지 정보를 넘겨 줍니다.
void OnDeleteMessage(BackndChat::MessageInfo messageInfo) { }
```

### 호출 함수

```cpp
// 채팅 메시지를 입력 하는 함수 입니다. 채널 그룹, 채널 이름, 채널 번호, 메시지를 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendChatMessage(string channelGroup, string channelName, uint64_t channelNumber, string message);

// 귓속말을 입력 하는 함수입니다. 유저 이름(귓속말 보낼), 메시지를 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendWhisperMessage(string gamerName, string message);
```

### 정보 클래스

#### MESSAGE_TYPE (enum)

| Value          | Description      |
| :------------- | :--------------- |
| SYSTEM_MESSAGE | 관리자 메시지    |
| NORMAL_MESSAGE | 일반 채팅 메시지 |

```cpp
struct MessageInfo
{
  // 채널 그룹 이름
  string ChannelGroup;

  // 채널 이름
  string ChannelName;

  // 채널 번호
  uint64_t ChannelNumber;

  // 유저 이름
  string GamerName;

  // 아바타 이름
  string Avatar;

  // 채팅 인덱스
  uint64_t Index;

  // 채팅 메시지 타입
  MESSAGE_TYPE MessageType;

  // 채팅 메시지
  string Message;

  // 채팅 시간 정보(UTC 기준) yyyy-MM-dd HH:mm:ss  ex) 2020-01-01 00:00:00
  string Time;

  // 메시지 태그
  string Tag;
};
```

```cpp
struct MessageInfo
{
  // 귓속말 인덱스
  uint64_t Index;

  // 귓속말 보낸 유저 이름
  string FromGamerName;

  // 귓속말 보낸 유저 아바타 이름
  string FromAvatar;

  // 귓속말 메시지
  string Message;

  // 귓속말 시간 정보(UTC 기준) yyyy-MM-dd HH:mm:ss  ex) 2020-01-01 00:00:00
  string Time;

  // 메시지 태그
  string Tag
};
```
