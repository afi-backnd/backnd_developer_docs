---
sidebar_label: 채널
description: "채널"
---

# 채널

채널 입장, 채널 퇴장을 구현할 수 있고, 새로운 유저가 들어오거나 나갈 때 콜백을 이용할 수 있습니다.  
:::tip

콘솔에서 유저의 국가(`country`), 언어(`language`), 국가 코드(`countrycode`), 또는 길드(`guild`)를 통해 채널 그룹을 설정했다면, 유저가 채팅 서버에 접속할 때 해당 속성에 따라 자동으로 채널에 입장합니다. 자동 채널 그룹을 설정하려면 [콘솔 가이드 문서](/guide/console-guide/chat/channels#예약어-채널)를 확인하세요.

Unreal SDK는 커스텀 인증을 사용하며, 커스텀 인증 서버의 응답에는 국가·길드 정보가 없으므로 언어(`language`) 채널 그룹만 유저 정보에 따라 입장합니다. 국가(`country`)·국가 코드(`countrycode`) 채널 그룹은 해당 그룹의 기본(Fallback) 채널로 입장하며, 길드(`guild`) 채널에는 자동으로 입장하지 않습니다.

:::

:::caution

`language`, `country`, `countrycode`, `guild`, `whisper` 는 채널 그룹 이름으로 사용할 수 없습니다. 채널 자동 생성 기능에서 사용되는 예약어입니다.

:::

## 설명

채널 그룹으로 총 3가지가 있습니다. 오픈 채널, 프라이빗 채널, 길드 채널입니다. 길드 채널은 베이스의 길드 기능과 연동하여 사용하실 수 있습니다.
- 오픈 채널: 누구나 입장할 수 있는 공개 채널입니다. 채널 그룹과 채널 이름으로 입장합니다.
- 프라이빗 채널: 비밀번호가 있어야 입장할 수 있는 비공개 채널입니다.(비밀번호를 설정 하지 않을 경우 누구나 입장 가능) 채널 그룹과 채널 번호로 입장합니다.
- 길드 채널: 베이스의 길드 기능과 연동된 채널입니다. 길드에 가입된 유저만 입장할 수 있으며 자동으로 입장됩니다. 커스텀 인증을 사용하는 Unreal SDK에서는 사용할 수 없으므로, 아래의 프라이빗 채널을 이용한 방법으로 구현하세요.

:::info 채널 이름 중복 허용
채널 그룹으로 우선 구분을 하기 때문에 채널 이름이 중복 될 수 있습니다. 또한, 프라이빗 채널은 룸 개념과 동일하다고 생각하시면 됩니다.  
예를 들어, 채널 그룹이 `party`이고 채널 이름이 `보스 잡으러 가실 분?`인 프라이빗 채널이 여러개 있을 수 있습니다. 이런 식으로 단발성 채팅방으로 사용 할 수 있기 때문에 채널 이름 중복이 허용됩니다.
:::

아래는 채널에 관련된 콜백 및 호출 함수, 정보 클래스입니다.

### 콜백 함수

```cpp
// 채널 입장 완료 시 호출
void OnJoinChannel(BackndChat::ChannelInfo channelInfo) 
{
    // channelInfo: 채널 정보, 유저 목록, 이전 메시지 포함
}

// 명시적으로 채널을 나가거나, 연결이 끊겨 재연결을 시작하거나, 중복 접속(DUPLICATE_CONNECTION)으로 연결이 종료될 때 호출
void OnLeaveChannel(BackndChat::ChannelInfo channelInfo)
{
    // 재연결에 성공하면 같은 채널에 대해 OnJoinChannel이 다시 호출될 수 있습니다.
    // BackndChat::BDChatMain::Shutdown() 호출 시에는 호출되지 않습니다.
}

// 다른 유저가 채널에 입장할 때 호출
void OnJoinChannelPlayer(
    std::string channelGroup, 
    std::string channelName, 
    uint64_t channelNumber, 
    BackndChat::PlayerInfo player)
{
    // player: 입장한 유저 정보
}

// 다른 유저가 채널에서 퇴장할 때 호출
void OnLeaveChannelPlayer(
    std::string channelGroup, 
    std::string channelName, 
    uint64_t channelNumber, 
    BackndChat::PlayerInfo player)
{
    // player: 퇴장한 유저 정보
}
```

:::caution 호출 시점
채널 생성 및 입장 함수는 채팅 서버 연결이 완료된 상태에서만 처리됩니다. `Initialize` 직후나 연결이 끊긴 동안 호출하면 요청이 처리되지 않을 수 있으므로, 기본 채널의 `OnJoinChannel` 콜백 등 연결 완료 이후 시점에 호출하세요.
:::

### 호출 함수

```cpp
// 프라이빗 채널 타입 만 SDK에서 채널을 생성 하실 수 있습니다.
// 다른 채널 타입은 콘솔을 이용해 주시길 바랍니다.

// 프라이빗 채널을 생성 하는 함수 입니다. 채널 그룹, 채널 번호, 채널 이름, 최대 인원, 비밀번호을 넣어서 보내 줍니다.
// 비밀번호를 넣지 않을 경우 공개 채널로 생성 됩니다.
// 채널 번호를 0으로 보낼 경우 서버에서 자동으로 채널 번호를 부여 합니다.
BackndChat::BDChatMain::SendCreatePrivateChannel(std::string channelGroup, uint64_t channelNumber = 0, std::string channelName = "default", uint32_t maxCount = 50, std::string password = "");

// 오픈 채널 타입의 채널 입장 함수 입니다. 채널 그룹, 채널 이름을 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendJoinOpenChannel(std::string channelGroup, std::string channelName);

// 프라이빗 채널 타입의 채널 입장 함수 입니다. 채널 그룹, 채널 번호를 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendJoinPrivateChannel(std::string channelGroup, uint64_t channelNumber, std::string password = "");

// 채널에서 나가는 함수 입니다. 채널 그룹, 채널 이름, 0이 아닌 채널 번호를 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendLeaveChannel(std::string channelGroup, std::string channelName, uint64_t channelNumber);
```

:::info 길드 채널은 아래의 방법으로 구현 하셔야 합니다.

`SendJoinGuildChannel()`, `SendLeaveGuildChannel()`은 베이스의 길드 기능과 연동된 함수이므로 커스텀 인증을 사용하는 Unreal SDK에서는 동작하지 않습니다.

길드를 생성 시 아래의 코드를 입력 하세요. 단, `guild`는 자동 채널 생성 기능에서 사용하고 있는 예약어이기 때문에, `guild`가 아닌 다른 채널 그룹 이름을 사용해야 합니다. 생성이 완료 되면 자동으로 채널에 접속 합니다.

```cpp
BackndChat::BDChatMain::SendCreatePrivateChannel("guilds", 1, "guildname", 100, "");
```

이후에 재접속 후에는 아래의 함수를 호출 해 주셔야 길드 채널에 접속 하실 수 있습니다.

```cpp
BackndChat::BDChatMain::SendJoinPrivateChannel("guilds", 1);
```

길드원의 경우 즉시 가입, 또는 가입 수락 시 콜백이 구현 된 경우에도 위의 함수를 호출 해 주시면 됩니다.

길드원이 탈퇴 및 추방 콜백이 구현 된 경우는 아래의 함수를 호출 해 주셔야 길드 채널에서 나가집니다.

```cpp
BackndChat::BDChatMain::SendLeaveChannel("guilds", "guildname", 1);
```

:::

### 정보 클래스

#### ChannelInfo
채널 입장 시 제공되는 채널 정보입니다.
```cpp
struct ChannelInfo
{
  // 채널 그룹 이름
  std::string ChannelGroup;

  // 채널 이름
  std::string ChannelName;

  // 채널 번호
  uint64_t ChannelNumber;

  // 채널 최대 인원
  uint32_t MaxCount;

  // 현재 접속 중인 플레이어 정보
  std::map<std::string, PlayerInfo> Players;

  // 이전 채팅 메시지 리스트
  std::vector<MessageInfo> Messages;
};
```
#### PlayerInfo
채널 내 유저 정보입니다.
```cpp
struct PlayerInfo
{
  // 플레이어 이름
  std::string GamerName;

  // 플레이어 아바타 이름
  std::string Avatar;

  // 플레이어 언어 정보
  std::string Language;

  // 플레이어 메타 정보
  std::map<std::string, std::string> Metadata;
};
```

### 이전 메시지 확인

```cpp
void ChatClientManager::OnJoinChannel(BackndChat::ChannelInfo channelInfo)
{
    // 이전 메시지 확인
    for (const BackndChat::MessageInfo& message : channelInfo.Messages)
    {
        UE_LOG(LogTemp, Log, TEXT("[이전메시지] %s: %s"), UTF8_TO_TCHAR(message.GamerName.c_str()), UTF8_TO_TCHAR(message.Message.c_str()));
        UE_LOG(LogTemp, Log, TEXT("전송시각: %s"), UTF8_TO_TCHAR(message.Time.c_str()));
    }

    // 현재 접속중인 유저 확인
    for (const auto& pair : channelInfo.Players)
    {
        UE_LOG(LogTemp, Log, TEXT("접속중: %s"), UTF8_TO_TCHAR(pair.second.GamerName.c_str()));
    }
}
```
