---
sidebar_label: 채널
description: "채널"
---

# 채널

채널 입장, 채널 퇴장을 구현할 수 있고, 새로운 유저가 들어오거나 나갈 때 콜백을 이용할 수 있습니다.  
:::tip

콘솔에서 유저의 국가(`country`), 언어(`language`), 국가 코드(`countrycode`), 또는 길드(`guild`)를 통해 채널 그룹을 설정했다면, 유저가 채팅 서버에 접속할 때 해당 속성에 따라 자동으로 채널에 입장합니다. 자동 채널 그룹을 설정하려면 [콘솔 가이드 문서](/guide/console-guide/chat/channels#자동-채널-그룹)를 확인하세요.

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

```csharp
// 채널 입장 완료 시 호출
public void OnJoinChannel(ChannelInfo channelInfo)
{
    // channelInfo: 채널 정보, 유저 목록, 이전 메시지 포함
}

// 채널 퇴장 완료 시 호출
public void OnLeaveChannel(ChannelInfo channelInfo)
{
    // channelInfo: 퇴장한 채널의 정보
}

// 다른 유저가 채널에 입장할 때 호출
public void OnJoinChannelPlayer(
    string channelGroup, 
    string channelName, 
    UInt64 channelNumber, 
    PlayerInfo player)
{
    // player: 입장한 유저 정보
}

// 다른 유저가 채널에서 퇴장할 때 호출
public void OnLeaveChannelPlayer(
    string channelGroup, 
    string channelName, 
    UInt64 channelNumber, 
    PlayerInfo player)
{
    // player: 퇴장한 유저 정보
}
```

### 호출 함수

```csharp
// 프라이빗 채널 타입 만 SDK에서 채널을 생성 하실 수 있습니다.
// 다른 채널 타입은 콘솔을 이용해 주시길 바랍니다.

// 프라이빗 채널을 생성 하는 함수 입니다. 채널 그룹, 채널 번호, 채널 이름, 최대 인원, 비밀번호을 넣어서 보내 줍니다.
// 비밀번호를 넣지 않을 경우 공개 채널로 생성 됩니다.
// 채널 번호를 0으로 보낼 경우 서버에서 자동으로 채널 번호를 부여 합니다.
ChatClient.SendCreatePrivateChannel(string channelGroup, UInt64 channelNumber = 0, string channelName = "default", uint maxCount = 50, string password = "")

// 오픈 채널 타입의 채널 입장 함수 입니다. 채널 그룹, 채널 이름을 넣어서 보내 줍니다.
ChatClient.SendJoinOpenChannel(string channelGroup, string channelName)

// 프라이빗 채널 타입의 채널 입장 함수 입니다. 채널 그룹, 채널 번호를 넣어서 보내 줍니다.
ChatClient.SendJoinPrivateChannel(string channelGroup, ulong channelNumber, string password = "")

// 채널에서 나가는 함수 입니다. 채널 그룹, 채널 이름, 채널 번호를 넣어서 보내 줍니다.
ChatClient.SendLeaveChannel(string channelGroup, string channelName, UInt64 channelNumber = 0)
```

```csharp
// 뒤끝 베이스 길드 기능을 사용하고 계신 경우에만 사용 가능한 길드 채널 관련 함수 입니다.

// 길드 채널 입장 함수 입니다.
ChatClient.SendJoinGuildChannel();

// 길드 채널에서 나가는 함수 입니다.
ChatClient.SendLeaveGuildChannel();
```

:::info 베이스의 길드 기능을 사용 하고 계실 경우 신규 길드의 길드 채팅 채널 입장 및 나가기 로직을 따로 구현 하셔야 합니다.  
예를 들어 길드 생성(CreateGuildV3) 후 아래의 코드를 호출 해 주셔야 합니다.

```csharp
ChatClient.SendJoinGuildChannel();
```

길드 탈퇴(WithdrawGuildV3) 후에는 아래의 코드를 호출 해 주셔야 합니다.

```csharp
ChatClient.SendLeaveGuildChannel();
```

또한, 길드 가입 수락 처리를 실시간 알림으로 처리 하신 경우 OnApprovedGuildJoin 콜백이 올 경우에도 길드 채팅 채널 입장 함수를 호출 해 주셔야 합니다.
함수를 호출 하시면 자동으로 가입 된 길드의 길드 채팅 채널로 입장 됩니다.

만일 따로 로직 처리를 해 주시지 않으실 경우 유저가 재 접속 전까지는 길드 채팅을 이용 하실 수 없습니다.

OnGuildMemberExpelled 콜백이 올 경우에도 길드 채팅 채널 나가기 함수를 호출 해 주시길 바랍니다. [(실시간 알림)](/sdk-docs/backend/base/notify/handler-user/guild)
:::

:::info 채팅 SDK 단독으로 사용하여 길드 채널을 사용하시기 위해서는 아래의 방법으로 구현 하셔야 합니다.

길드를 생성 시 아래의 코드를 입력 하세요. 단, `guild`는 자동 채널 생성 기능에서 사용하고 있는 예약어이기 때문에, `guild`가 아닌 다른 채널 그룹 이름을 사용해야 합니다. 생성이 완료 되면 자동으로 채널에 접속 합니다.

```csharp
ChatClient.SendCreatePrivateChannel("guilds", 1, "guildname", 100, "");
```

이후에 재접속 후에는 아래의 함수를 호출 해 주셔야 길드 채널에 접속 하실 수 있습니다.

```csharp
ChatClient.SendJoinPrivateChannel("guilds", 1);
```

길드원의 경우 즉시 가입, 또는 가입 수락 시 콜백이 구현 된 경우에도 위의 함수를 호출 해 주시면 됩니다.

길드원이 탈퇴 및 추방 콜백이 구현 된 경우는 아래의 함수를 호출 해 주셔야 길드 채널에서 나가집니다.

```csharp
ChatClient.SendLeaveChannel("guilds", "guildname", 1);
```

:::

### 정보 클래스

#### ChannelInfo
채널 입장 시 제공되는 채널 정보입니다.
```csharp
public class ChannelInfo
{
    // 채널 정보
    public string ChannelGroup;    // 채널 그룹명
    public string ChannelName;     // 채널명
    public UInt64 ChannelNumber;   // 채널 번호
    public uint MaxCount;          // 최대 인원수

    // 채널 데이터
    public Dictionary<string, PlayerInfo> Players;   // 접속중인 유저 목록
    public List<MessageInfo> Messages;              // 이전 채팅 메시지
}
```

#### PlayerInfo
채널 내 유저 정보입니다.
```csharp
public class PlayerInfo
{
    public string GamerName;    // 유저명
    public string Avatar;       // 아바타
    public string Language;     // 언어
    public Dictionary<string, string> Metadata;   // 추가 정보
}
```

### 이전 메시지 확인

```csharp
public void OnJoinChannel(ChannelInfo channelInfo)
{
    // 이전 메시지 확인
    foreach (var msg in channelInfo.Messages)
    {
        Debug.Log($"[이전메시지] {msg.GamerName}: {msg.Message}");
    }

    // 현재 접속중인 유저 확인
    foreach (var player in channelInfo.Players)
    {
        Debug.Log($"접속중: {player.Value.GamerName}");
    }
}
```
