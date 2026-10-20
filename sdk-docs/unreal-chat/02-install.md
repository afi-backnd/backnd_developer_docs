---
sidebar_label: 설치 및 환경 구성
description: "설치 및 환경 구성"
---

# 설치 및 환경 구성

## SDK 다운로드

- <a href="https://developer.thebackend.io/sdk/chat/1.4.4/BackndChat-1.4.4-UE53.zip" id="download-sdk-chat">BackndChat-1.4.4-UE53.zip</a>&nbsp;[2026-10-20]

:::info Unreal Engine 4.27
- <a href="https://developer.thebackend.io/sdk/chat/1.4.4/BackndChat-1.4.4-UE427.zip" id="download-sdk-chat">BackndChat-1.4.4-UE427.zip</a>&nbsp;[2026-10-20]
:::

## 지원 플랫폼 및 요구 사양

| 플랫폼 | Unreal Engine 5.3 | Unreal Engine 4.27 |
|---|---|---|
| Windows | x64 | x64 |
| macOS | Apple Silicon, Intel | Apple Silicon, Intel |
| Linux | x86_64 | x86_64 |
| Android | arm64-v8a, x86_64, **API 26** 이상, NDK r25b | armeabi-v7a, arm64-v8a, x86_64, **API 21** 이상, NDK r21b~r23a |
| iOS | arm64, **iOS 15.0** 이상 | arm64, **iOS 15.0** 이상 |

SDK 라이브러리는 Android는 NDK r25b(Unreal Engine 5.3)·r21e(Unreal Engine 4.27), iOS는 Xcode 27.1로 빌드했습니다.

## 채팅 활성화 하기

콘솔에서 채팅을 활성화하는 방법은 다음과 같습니다.

### 1. 콘솔에 접속합니다.

### 2. 채팅 아래의 채널 또는 신고 및 제재 또는 설정을 클릭합니다.

![image](/img/docs/guide/chat/install/console.png)

### 3. 채팅 사용하기 버튼을 클릭합니다.

![image](/img/docs/guide/chat/install/new-chat.png)

채팅을 활성화하면 `global` 그룹의 `server-1` 기본 채널이 자동으로 생성됩니다. 추가 채널이 필요한 경우에만 아래와 같이 채널을 생성합니다.

## 채팅 채널 생성 하기

콘솔에서 채팅 -> 채널에서 채팅 채널을 생성 하실 수 있습니다.

### 1. 채널을 클릭 합니다.

![channel empty](/img/docs/guide/chat/console/channel-empty.png)

### 2. 채널그룹 및 채널 생성하기

1. 채널 그룹 옆 + 버튼을 클릭 합니다.

![channel type](/img/docs/guide/chat/console/channel-create-type.png)

2. 채널 그룹 이름 과 채널당 최대 인원을 입력 후 Fallback 채널을 선택 하고 다음 버튼을 클릭 합니다.

![create open channel group](/img/docs/guide/chat/console/channel-create-open-channel-group.png)

3. 첫 번째 채널 이름을 입력 후 생성하기 버튼을 클릭 합니다.

![create open channel](/img/docs/guide/chat/console/channel-create-open-channel.png)

![created open channel](/img/docs/guide/chat/console/channel-created-open.png)

## 설치 및 환경 구성

채팅 SDK를 다운로드하고 Unreal Engine에서 프로젝트를 구성하는 방법은 다음과 같습니다.

1. 프로젝트가 위치한 폴더로 이동 합니다.
2. Plugins 폴더에 다운로드 받은 SDK 파일의 압축을 해제 합니다. (Plugins 폴더가 없다면 생성 하시길 바랍니다.)
3. Unreal Engine을 실행 하여 편집 -> 플로그인 -> Other 를 확인 해 주시길 바랍니다.
4. 플러그인이 적용 되어 있는 지 체크 박스를 확인 해 주시길 바랍니다.

![image](/img/docs/guide/chat/unreal/plugin.png)

## 커스텀 인증으로 변경 및 채팅 서버 키 확인
뒤끝 콘솔에서 채팅 > 설정 탭에서 유저 인증 방법을 커스텀 인증 사용하기로 변경해줍니다.  
해당 설정에 관련된 내용은 [커스텀 인증](/sdk-docs/unreal-chat/custom-auth) 문서를 참고해주시기 바랍니다.

그리고 언리얼에 입력할 채팅 서버 키(Chat UUID)를 확인합니다.

![image](/img/docs/guide/chat/install/chat-inspector-chat-custom.png)


## Chat Client

### 설명

변경사항과 메시지를 받기 위해서 어플리케이션에 콜백으로 사용되고 있는 일부 메소드를 구현해야 합니다. BackndChat::IBDChatClientListener 인터페이스로 정의되어 있습니다.

### 특징

`BackndChat::BDChatMain::Update()`를 매 프레임 호출해야 수신 메시지와 콜백이 처리됩니다. 콜백은 `Update()` 또는 SDK 함수를 호출한 스레드에서 실행되므로 게임 스레드에서 호출해 주세요.

뒤끝 채팅 SDK는 네트워크 변경이나 일시적인 연결 끊김 상황에서도 자동으로 다시 연결되는 기능을 제공합니다. 별도의 재연결 처리는 필요하지 않으며, 최초 한번 `BackndChat::BDChatMain::Initialize(this, args);`를 호출한 뒤 종료 시점에는 `BackndChat::BDChatMain::Shutdown();`을 호출하세요.

- 연결이 끊기면 입장해 있던 채널마다 `OnLeaveChannel`이 호출됩니다. 재연결에 성공하면 같은 채널에 대해 `OnJoinChannel`이 다시 호출될 수 있습니다.
- 재연결은 1초 후 처음 시도하며, 이후 10초, 20초, ... 순으로 대기 시간이 늘어나 최대 120초까지 대기합니다.
- 앱 일시정지나 레벨 로딩 등으로 `Update()`가 10초를 넘게 호출되지 않다가 다시 호출되면 재연결 대기 시간을 초기화합니다. 재연결을 기다리는 중이었다면 바로 재연결을 시도합니다.
- 연결이 끊겨 재연결을 기다리는 동안 호출한 요청은 서버로 전송되지 않습니다. 재연결 시도 중에 호출한 요청은 전송될 수도 있으므로, 요청은 `OnJoinChannel` 등 연결이 완료된 이후에 호출하세요. 재연결 전까지 처리하지 못한 이전 연결의 수신 메시지는 콜백으로 전달되지 않습니다.
- `Update()`가 호출되지 않아 처리하지 못한 수신 메시지가 1,000건 또는 8MB를 넘으면 연결을 다시 맺습니다.

:::caution 1.4.4 변경 사항
1.4.4부터 SDK 헤더가 전역 `using namespace std;`를 선언하지 않습니다. `string`, `vector`, `map`을 `std::` 없이 사용하던 코드는 수정해야 합니다. 자세한 내용은 [마이그레이션 가이드](/sdk-docs/unreal-chat/migration-guide)를 참고하세요.
:::

### Example

```cpp
// ChatClientManager.h

#include "BDChatMain.h"

class ChatClientManager : public BackndChat::IBDChatClientListener
{
public:
	ChatClientManager();
	virtual ~ChatClientManager();

public:
	virtual void OnJoinChannel(BackndChat::ChannelInfo channelInfo) override;
	virtual void OnLeaveChannel(BackndChat::ChannelInfo channelInfo) override;
	virtual void OnJoinChannelPlayer(std::string channelGroup, std::string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) override;
	virtual void OnLeaveChannelPlayer(std::string channelGroup, std::string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) override;
	virtual void OnUpdatePlayerInfo(std::string channelGroup, std::string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) override;
	virtual void OnChangeGamerName(std::string oldGamerName, std::string newGamerName) override;
	virtual void OnChatMessage(BackndChat::MessageInfo messageInfo) override;
	virtual void OnWhisperMessage(BackndChat::WhisperMessageInfo messageInfo) override;
	virtual void OnTranslateMessage(std::map<std::string, BackndChat::MessageInfo> messages) override;
	virtual void OnHideMessage(BackndChat::MessageInfo messageInfo) override;
	virtual void OnDeleteMessage(BackndChat::MessageInfo messageInfo) override;
	virtual void OnSuccess(BackndChat::SUCCESS_MESSAGE success, void* param) override;
	virtual void OnError(BackndChat::ERROR_MESSAGE error, void* param) override;

public:
	// 매 프레임 호출해 주세요. (예: Actor 또는 HUD의 Tick에서 호출)
	void Tick(float DeltaTime);
};

// ChatClientManager.cpp

#include "ChatClientManager.h"

ChatClientManager::ChatClientManager()
{
	BackndChat::BDChatClientArguments args;
	args.UUID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";
	args.CustomAccessToken = USER_TOKEN;
	args.Avatar = "default";
	args.Metadata.insert(std::make_pair("key1", "value1"));
	args.Metadata.insert(std::make_pair("key2", "value2"));
	args.Metadata.insert(std::make_pair("key3", "value3"));

	BackndChat::BDChatMain::Initialize(this, args);
}

ChatClientManager::~ChatClientManager()
{
	BackndChat::BDChatMain::Shutdown();
}

void ChatClientManager::OnJoinChannel(BackndChat::ChannelInfo channelInfo) { }

void ChatClientManager::OnLeaveChannel(BackndChat::ChannelInfo channelInfo) { }

void ChatClientManager::OnJoinChannelPlayer(std::string channelGroup, std::string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) { }

void ChatClientManager::OnLeaveChannelPlayer(std::string channelGroup, std::string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) { }

void ChatClientManager::OnUpdatePlayerInfo(std::string channelGroup, std::string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) { }

void ChatClientManager::OnChangeGamerName(std::string oldGamerName, std::string newGamerName) { }

void ChatClientManager::OnChatMessage(BackndChat::MessageInfo messageInfo) { }

void ChatClientManager::OnWhisperMessage(BackndChat::WhisperMessageInfo messageInfo) { }

void ChatClientManager::OnTranslateMessage(std::map<std::string, BackndChat::MessageInfo> messages) { }

void ChatClientManager::OnHideMessage(BackndChat::MessageInfo messageInfo) { }

void ChatClientManager::OnDeleteMessage(BackndChat::MessageInfo messageInfo) { }

void ChatClientManager::OnSuccess(BackndChat::SUCCESS_MESSAGE success, void* param)
{
	switch(success)
	{
		default:
			break;
	}
}

void ChatClientManager::OnError(BackndChat::ERROR_MESSAGE error, void* param)
{
	switch(error)
	{
		default:
			break;
	}
}

void ChatClientManager::Tick(float DeltaTime)
{
	BackndChat::BDChatMain::Update();
}
```

## 메시지 보내기

뒤끝 채팅을 활성화하면 `global` 그룹의 `server-1` 기본 채널이 자동으로 생성됩니다.
기본 채널 입장이 완료되어 `OnJoinChannel` 콜백이 오면 메시지를 전송합니다.

```cpp {3,5}
void ChatClientManager::OnJoinChannel(BackndChat::ChannelInfo channelInfo)
{
	if (channelInfo.ChannelGroup != "global" || channelInfo.ChannelName != "server-1") return;

	BackndChat::BDChatMain::SendChatMessage(channelInfo.ChannelGroup, channelInfo.ChannelName, channelInfo.ChannelNumber, TCHAR_TO_UTF8(TEXT("안녕하세요!")));
}
```

:::caution 호출 시점
`BackndChat::BDChatMain::Initialize` 호출 직후에는 아직 채팅 서버에 연결되지 않았으므로 채널 입장 요청이 처리되지 않을 수 있습니다. 추가 채널 입장 함수는 기본 채널의 `OnJoinChannel` 콜백 등 채팅 서버 연결이 완료된 이후 호출하세요.
:::

## 콘솔에서 메시지 확인하기

뒤끝 콘솔에서 `server-1` 채널을 선택하면, 클라이언트에서 보낸 채팅 메시지를 확인할 수 있습니다.
![first message](/img/docs/guide/chat/install/first-message.png)

## 채팅 연동이 완료되었습니다

이제 실시간 채팅 기능을 게임에서 사용할 수 있게 되었습니다.  
더 확장하거나 실제 프로젝트에 적용하려면 아래 문서들을 참고하세요.

- **UI까지 완성된 샘플을 보고 싶어요** → [예제 프로젝트](/sdk-docs/unreal-chat/example-game)
- **다른 채팅 기능이 궁금해요** → [채팅 기능 소개](/guide/getting-started/how-to-use/backnd-chat)
- **커스텀 인증 서버를 구성하고 싶어요** → [커스텀 인증 가이드](/sdk-docs/unreal-chat/custom-auth)
