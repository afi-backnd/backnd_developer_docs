---
sidebar_label: 설치 및 환경 구성
description: "설치 및 환경 구성"
---

# 설치 및 환경 구성

## SDK 다운로드

- <a href="https://developer.thebackend.io/sdk/chat/1.3.0/BackndChat-1.3.0-UE53.zip" id="download-sdk-chat">BackndChat-1.3.0-UE53.zip</a>&nbsp;[2024-10-29]

:::info Supported Platforms
- Android, iOS, Windows, Mac, Linux
:::

:::info Unreal Engine 4.27
- <a href="https://developer.thebackend.io/sdk/chat/1.3.0/BackndChat-1.3.0-UE427.zip" id="download-sdk-chat">BackndChat-1.3.0-UE427.zip</a>&nbsp;[2024-10-29]
:::

## 채팅 활성화 하기

콘솔에서 채팅을 활성화하는 방법은 다음과 같습니다.

### 1. 콘솔에 접속합니다.

### 2. 채팅 아래의 채널 또는 신고 및 제재 또는 설정을 클릭합니다.

![image](/img/docs/guide/chat/install/console.png)

### 3. 채팅 사용하기 버튼을 클릭합니다.

![image](/img/docs/guide/chat/install/new-chat.png)

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

### 3. 콘솔에서 메시지 확인하기

뒤끝 콘솔에서 `server-1` 채널을 선택하면, 클라이언트에서 보낸 채팅 메시지를 확인할 수 있습니다.
![first message](/img/docs/guide/chat/install/first-message.png)

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

저희 채팅 SDK의 특징 중 자동 재 연결 프로세스가 있습니다. 유저의 네트워크가 변경 되거나 리스가 발생 할 경우 자동으로 재 연결 프로세스가 실행 됩니다.
자동 재 연결 프로세스가 존재 하여 최초 한번 BackndChat::BDChatMain::Initialize(this, args); 호출 후 앱 종료 처리 시 BackndChat::BDChatMain::Shutdown(); 호출 해 주신다면 따로 관리 하실 필요가 없습니다.

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
	virtual void OnJoinChannelPlayer(string channelGroup, string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) override;
	virtual void OnLeaveChannelPlayer(string channelGroup, string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) override;
	virtual void OnUpdatePlayerInfo(string channelGroup, string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) override;
	virtual void OnChangeGamerName(string oldGamerName, string newGamerName) override;
	virtual void OnChatMessage(BackndChat::MessageInfo messageInfo) override;
	virtual void OnWhisperMessage(BackndChat::WhisperMessageInfo messageInfo) override;
	virtual void OnTranslateMessage(std::vector<BackndChat::MessageInfo> messages) override;
	virtual void OnHideMessage(BackndChat::MessageInfo messageInfo) override;
	virtual void OnDeleteMessage(BackndChat::MessageInfo messageInfo) override;
	virtual void OnSuccess(BackndChat::SUCCESS_MESSAGE success, void* param) override;
	virtual void OnError(BackndChat::ERROR_MESSAGE error, void* param) override;
}

// ChatClientManager.cpp

#include "ChatClientManager.h"

ChatClientManager::ChatClientManager()
{
	BackndChat::BDChatClientArguments args;
	args.UUID = "xxxx-xxxx-xxxxxx-xxxxxxx";
	args.CustomAccessToken = USER_TOKEN
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

void ChatClientManager::OnJoinChannelPlayer(string channelGroup, string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) { }

void ChatClientManager::OnLeaveChannelPlayer(string channelGroup, string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) { }

void ChatClientManager::OnUpdatePlayerInfo(string channelGroup, string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) { }

void ChatClientManager::OnChangeGamerName(string oldGamerName, string newGamerName) { }

void ChatClientManager::OnChatMessage(BackndChat::MessageInfo messageInfo) { }

void ChatClientManager::OnWhisperMessage(BackndChat::WhisperMessageInfo messageInfo) { }

void ChatClientManager::OnTranslateMessage(std::vector<BackndChat::MessageInfo> messages) { }

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
