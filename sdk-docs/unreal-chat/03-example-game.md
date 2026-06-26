---
sidebar_label: 예제게임
---

# 예제 게임

## 개요

채팅 예제 게임은 채팅의 기능을 쉽게 이용해 볼 수 있도록, 예제 코드가 포함되어 있는 Unreal Engine 샘플 프로젝트입니다.

![image](/img/docs/guide/chat/unreal/example-game/example1.png)
![image](/img/docs/guide/chat/unreal/example-game/example2.png)

개발된 버전 정보는 다음과 같습니다.

- Unreal Engine 5.3.2

### 다운로드

- <a href="https://developer.thebackend.io/sdk/chat/1.3.0/BackndChatExample-UE53.zip" id="download-sdk-chat-example">BackndChatExample-UE53.zip</a>&nbsp;[2024-10-29]

:::info Supported Platforms
- Android, iOS, Windows, Mac, Linux
:::

:::info Unreal Engine 4.27
- <a href="https://developer.thebackend.io/sdk/chat/1.3.0/BackndChatExample-UE427.zip" id="download-sdk-chat-example">BackndChatExample-UE427.zip</a>&nbsp;[2024-10-29]
:::

## 설정 방법

### 1. 채팅 예제게임 실행

위 다운로드에서 **BackndChatExample-UE53.zip**를 다운받고 압축 해제 후 프로젝트를 실행 합니다.

![image](/img/docs/guide/chat/unreal/example-game/setting1.png)

### 2. 콘솔에서 채팅 활성화

뒤끝 콘솔의 **채팅 > 설정**에서 **채팅 사용하기**를 클릭합니다.

![image](/img/docs/guide/chat/example-game/setting3.png)


### 3. 채널그룹 및 채널 생성하기

1. 채널 그룹 옆 + 버튼을 클릭 합니다.
   ![channel empty](/img/docs/guide/chat/console/channel-empty.png)

2. 오픈 타입을 클릭 후 다음 버튼을 클릭 합니다.

![channel type](/img/docs/guide/chat/console/channel-create-type.png)

3. 채널 그룹 이름 과 채널당 최대 인원을 입력 후 Fallback 채널을 선택 하고 다음 버튼을 클릭 합니다.

![create open channel group](/img/docs/guide/chat/console/channel-create-open-channel-group.png)

4. 첫 번째 채널 이름을 입력 후 생성하기 버튼을 클릭 합니다.

![create open channel](/img/docs/guide/chat/console/channel-create-open-channel.png)

![created open channel](/img/docs/guide/chat/console/channel-created-open.png)

### 4. 뒤끝 콘솔 프로젝트의 채팅 서버 키 복사

뒤끝 콘솔에 **채팅 > 설정**에서 **Chat UUID**를 복사합니다.

![image](/img/docs/guide/chat/install/chat-inspector-chat.png)

### 5. 프로젝트 실행

설정이 모두 완료되었습니다. 
 ![image](/img/docs/guide/chat/unreal/example-game/example1.png)

### 6. 예제게임 명령어

:::info
필요 없는 파라미터일 경우 공백으로 비워 두시길 바랍니다.
:::

```
/login xxxx-xxxx-xxxxxx-xxxxxxx a0 a0 ko : 로그인 (UUID, ID, PW, Language)

/cc test1 test1 0 password 5000 : 프라이빗 채널 생성 (채널 그룹, 채널 이름, 채널 번호, 패스워드, 최대 인원)

/oj test1 test1 : 오픈 채널 입장 (채널 그룹, 채널 이름)

/pj test1 1 password : 프라이빗 채널 입장 (채널 그룹, 채널 번호, 채널 비밀 번호)

/l test1 test1 1 : 채널 퇴장 (채널 그룹, 채널 이름, 채널 번호)

/w a1 message : 귓속말 (유저 이름, 메세지)

/report 1 reason : 메세지 신고하기 (메세지 인덱스 번호, 신고 사유 : 빈값인 경우 설정 한 사유)

/translate en : 온디맨드 번역 기능 (번역할 언어 : en / ko / ja)

/info a1 : 메타 정보 가져오기 (유저 이름)

/nickname a222222 : 닉네임 변경 (유저 이름)

/meta add test test : 메타 정보 추가 (key, value)

/meta remove test : 메타 정보 삭제 (key)

/meta update test test111 : 메타 정보 업데이트 (key, value)

/language ko-KR : 언어 정보 변경 (변경할 언어)

/avatar Boy_1 : 아바타 변경 (아바타 이름)
```
