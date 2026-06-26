---
sidebar_label: 채널
description: "채널"
---

# 채널

채널 입장, 채널 퇴장을 구현할 수 있고, 새로운 유저가 들어오거나 나갈 때 콜백을 이용할 수 있습니다.  

:::tip

콘솔에서 언어(`language`) 채널 그룹을 설정했다면, 유저가 채팅 서버에 접속할 때 언어에 따라 자동으로 채널에 입장합니다. 자동 채널 그룹을 설정하려면 [콘솔 가이드 문서](/guide/console-guide/chat/channels#자동-채널-그룹)를 확인하세요.

:::

:::caution

`language`, `country`, `countrycode`, `guild`, `whisper` 는 채널 그룹 이름으로 사용할 수 없습니다. 채널 자동 생성 기능에서 사용되는 예약어입니다.

:::

## 설명

채널 그룹으로 총 3가지가 있습니다. 오픈 채널, 프라이빗 채널, 길드 채널입니다. 길드 채널은 베이스의 길드 기능과 연동하여 사용하실 수 있습니다.
- 오픈 채널: 누구나 입장할 수 있는 공개 채널입니다. 채널 그룹과 채널 이름으로 입장합니다.
- 프라이빗 채널: 비밀번호가 있어야 입장할 수 있는 비공개 채널입니다.(비밀번호를 설정 하지 않을 경우 누구나 입장 가능) 채널 그룹과 채널 번호로 입장합니다.
- 길드 채널: 베이스의 길드 기능과 연동된 채널입니다. 길드에 가입된 유저만 입장할 수 있으며 자동으로 입장됩니다.

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

// 채널 퇴장 완료 시 호출
void OnLeaveChannel(BackndChat::ChannelInfo channelInfo)
{
    // channelInfo: 퇴장한 채널의 정보
}

// 다른 유저가 채널에 입장할 때 호출
void OnJoinChannelPlayer(
    string channelGroup, 
    string channelName, 
    uint64_t channelNumber, 
    BackndChat::PlayerInfo player)
{
    // player: 입장한 유저 정보
}

// 다른 유저가 채널에서 퇴장할 때 호출
void OnLeaveChannelPlayer(
    string channelGroup, 
    string channelName, 
    uint64_t channelNumber, 
    BackndChat::PlayerInfo player)
{
    // player: 퇴장한 유저 정보
}
```

### 호출 함수

```cpp
// 프라이빗 채널 타입 만 SDK에서 채널을 생성 하실 수 있습니다.
// 다른 채널 타입은 콘솔을 이용해 주시길 바랍니다.

// 프라이빗 채널을 생성 하는 함수 입니다. 채널 그룹, 채널 번호, 채널 이름, 최대 인원, 비밀번호을 넣어서 보내 줍니다.
// 비밀번호를 넣지 않을 경우 공개 채널로 생성 됩니다.
// 채널 번호를 0으로 보낼 경우 서버에서 자동으로 채널 번호를 부여 합니다.
BackndChat::BDChatMain::SendCreatePrivateChannel(string channelGroup, uint64_t channelNumber = 0, string channelName = "default", uint32_t maxCount = 50, string password = "");

// 오픈 채널 타입의 채널 입장 함수 입니다. 채널 그룹, 채널 이름을 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendJoinOpenChannel(string channelGroup, string channelName);

// 프라이빗 채널 타입의 채널 입장 함수 입니다. 채널 그룹, 채널 번호를 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendJoinPrivateChannel(string channelGroup, uint64_t channelNumber, string password = "");

// 채널에서 나가는 함수 입니다. 채널 그룹, 채널 이름, 채널 번호를 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendLeaveChannel(string channelGroup, string channelName, uint64_t channelNumber);
```

### 정보 클래스

#### ChannelInfo
채널 입장 시 제공되는 채널 정보입니다.
```cpp
struct ChannelInfo
{
  // 채널 그룹 이름
  string ChannelGroup;

  // 채널 이름
  string ChannelName;

  // 채널 번호
  uint64_t ChannelNumber;

  // 채널 최대 인원
  uint32_t MaxCount;

  // 현재 접속 중인 플레이어 정보
  map<string, PlayerInfo> Players;

  // 이전 채팅 메시지 리스트
  vector<MessageInfo> Messages;
};
```
#### PlayerInfo
채널 내 유저 정보입니다.
```cpp
struct PlayerInfo
{
  // 플레이어 이름
  string GamerName;

  // 플레이어 아바타 이름
  string Avatar;

  // 플레이어 언어
  string Language;

  // 플레이어 메타 정보
  map<string, string> Metadata;
};
```

### 이전 메시지 확인

```cpp
void UChatManager::OnJoinChannel(const FChannelInfo& ChannelInfo)
{
    // 이전 메시지 확인
    for (const FMessageInfo& Msg : ChannelInfo.Messages)
    {
        UE_LOG(LogTemp, Log, TEXT("[이전메시지] %s: %s"), 
            *Msg.GamerName, *Msg.Message);
        UE_LOG(LogTemp, Log, TEXT("전송시각: %s"), *Msg.SendTime);
    }

    // 현재 접속중인 유저 확인
    for (const TPair<FString, FPlayerInfo>& Pair : ChannelInfo.Players)
    {
        UE_LOG(LogTemp, Log, TEXT("접속중: %s"), 
            *Pair.Value.GamerName);
    }
}
```
