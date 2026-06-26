---
sidebar_label: 예제게임
description: "예제게임"
---

# 예제 게임

## 개요

채팅 예제 게임은 채팅의 기능을 쉽게 이용해 볼 수 있도록, 예제 코드가 포함되어 있는 유니티 샘플 프로젝트입니다.

![image](/img/docs/guide/chat/example-game/example1.png)
![image](/img/docs/guide/chat/example-game/example2.png)
![image](/img/docs/guide/chat/example-game/example3.png)

개발된 버전 정보는 다음과 같습니다.

- Unity 2022.3.13f1
- Backend 5.11.9

### 다운로드

- <a href="https://developer.thebackend.io/sdk/chat/1.4.0/BackndChatExample.unitypackage" id="download-sdk-chat-example">BackndChatExample.unitypackage</a>&nbsp;[2025-12-18]

:::info Supported Platforms
- Android, iOS, Windows, Mac, Linux
:::

## 설정 방법

### 1. 채팅 예제게임 import

위 다운로드에서 **BackndChatExample.unitypackage**를 다운받고 import 합니다.

![image](/img/docs/guide/chat/example-game/setting1.png)

### 2. 콘솔에서 채팅 활성화

뒤끝 콘솔의 **채팅 > 설정**에서 **채팅 사용하기**를 클릭합니다.

![image](/img/docs/guide/chat/example-game/setting3.png)



### 3. 뒤끝 콘솔 프로젝트의 공통 서버 키 입력

1. 뒤끝 콘솔에서 **채팅 > 설정** 탭으로 이동합니다. 공통 서버 키를 복사합니다.

![image](/img/docs/guide/chat/install/chat-inspector-base.png)

2. 유니티 상단에 The Backend > Edit Settings를 클릭합니다. 생성된 인스펙터에서 Client App ID와 Signature Key를 입력합니다.

![image](/img/docs/guide/chat/install/chat-inspector-base-unity.png)


### 4. 뒤끝 콘솔 프로젝트의 채팅 서버 키 입력

뒤끝 콘솔에 **채팅 > 설정**에서 **Chat UUID**를 복사합니다.

![image](/img/docs/guide/chat/install/chat-inspector-chat.png)

유니티 상단에 The Backend > Edit Chat Settings를 클릭합니다. 생성된 인스펙터에서 Chat UUID를 입력합니다.

![image](/img/docs/guide/chat/install/chat-inspector-chat-unity.png)


### 5. 채널그룹 및 채널 생성하기

1. 채널 그룹 옆 + 버튼을 클릭 합니다.
   ![channel empty](/img/docs/guide/chat/console/channel-empty.png)

2. 오픈 타입을 클릭 후 다음 버튼을 클릭 합니다.

![channel type](/img/docs/guide/chat/console/channel-create-type.png)

3. 채널 그룹 이름 과 채널당 최대 인원을 입력 후 Fallback 채널을 선택 하고 다음 버튼을 클릭 합니다.

![create open channel group](/img/docs/guide/chat/console/channel-create-open-channel-group.png)

4. 첫 번째 채널 이름을 입력 후 생성하기 버튼을 클릭 합니다.

![create open channel](/img/docs/guide/chat/console/channel-create-open-channel.png)

![created open channel](/img/docs/guide/chat/console/channel-created-open.png)

### 6. 프로젝트 실행

설정이 모두 완료되었습니다. 유니티 프로젝트를 실행합니다.  
 ![image](/img/docs/guide/chat/example-game/example1.png)
