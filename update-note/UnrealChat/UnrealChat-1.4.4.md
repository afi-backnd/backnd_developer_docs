---
title: UnrealChat-1.4.4
date: 2026-10-20T10:00
slug: unreal-chat-1-4-4
---

:::info 업데이트 요약
유저 간 차단 기능에 차단 시간이 추가되었습니다.  
온디맨드 번역 결과를 번역 도착 언어별로 확인할 수 있도록 `OnTranslateMessage` 콜백이 변경되었습니다.  
재연결 및 연결 유지 안정성이 개선되었습니다.  
iOS 26 이상에서 앱이 종료될 수 있던 문제가 수정되었고, iOS 최소 지원 버전이 15.0으로 변경되었습니다.
:::

:::danger 함수 변경 안내
아래 함수가 변경되어 기존 코드를 그대로 사용하는 경우 컴파일 에러가 발생하니, [마이그레이션 가이드](/sdk-docs/unreal-chat/migration-guide)를 참고하여 반드시 수정해 주세요.

- `IBDChatClientListener::OnTranslateMessage`의 매개변수가 `std::vector<MessageInfo>`에서 `std::map<std::string, MessageInfo>`로 변경되었습니다. ([번역](/sdk-docs/unreal-chat/translate))
- `BDChatMain::GetBlockGamers()`의 반환 형식이 `std::vector<std::string>`에서 `std::vector<BlockPlayerInfo>`로 변경되었습니다. ([유저](/sdk-docs/unreal-chat/user))
- SDK 헤더가 전역 `using namespace std;`를 선언하지 않습니다. `string`, `vector`, `map`을 `std::` 없이 사용하던 코드는 `std::`를 붙여 주세요.
:::

:::caution iOS 최소 지원 버전 변경
포함된 iOS 라이브러리를 Xcode 27.1로 다시 빌드하여 iOS 최소 지원 버전이 12.0에서 15.0으로 변경되었습니다.  
Unreal Engine 4.27은 프로젝트 설정에서 최소 iOS 버전을 14까지만 선택할 수 있으며, iOS 15 미만 기기에서는 앱이 정상 동작하지 않을 수 있습니다.
:::

<!--truncate-->

[Unity SDK] <a href="https://developer.thebackend.io/sdk/chat/1.4.4/BackndChat-1.4.4.unitypackage" id="download-sdk-chat">다운로드</a>  
[Unreal 5.3 SDK] <a href="https://developer.thebackend.io/sdk/chat/1.4.4/BackndChat-1.4.4-UE53.zip" id="download-sdk-chat">다운로드</a>  
[Unreal 4.27 SDK] <a href="https://developer.thebackend.io/sdk/chat/1.4.4/BackndChat-1.4.4-UE427.zip" id="download-sdk-chat">다운로드</a>  

## 1.4.4 Update
* [번역] `OnTranslateMessage` 콜백의 매개변수가 `std::map<std::string, MessageInfo>`로 변경되었습니다. 키는 요청한 번역 도착 언어 코드이며, 번역에 실패한 언어는 포함되지 않습니다. (언어별 번역 결과를 제공하지 않는 채팅 서버에서는 키가 `#0`, `#1`... 형식으로 전달됩니다.)
* [유저 간 차단] 차단 시간이 추가되었습니다. `GetBlockGamers()`가 닉네임(`GamerName`)과 차단 시간(`BlockTime`)을 담은 `BlockPlayerInfo` 목록을 반환합니다.
* [유저 간 차단] 이미 차단한 유저를 차단하면 `ALREADY_BLOCK_PLAYER`, 차단하지 않은 유저를 해제하면 `FAILED_REMOVE_BLOCK_PLAYER` 에러를 요청 전송 없이 바로 전달합니다.
* [에러] 서버에서 전달한 `ALREADY_BLOCK_PLAYER`, `NOT_FOUND_BLOCK_URL`, `CHANNEL_GROUP_TOO_SHORT` 에러가 `UNKNOWN_ERROR`로 전달되던 문제가 수정되었습니다.
* [재연결] 첫 재연결은 1초 후에 시도하고, 이후에는 10초, 20초, ... 순으로 대기 시간이 늘어나 최대 120초까지 대기합니다.
* [재연결] 앱 일시정지나 로딩 등으로 `Update()`가 10초를 넘게 호출되지 않다가 다시 호출되면 재연결 대기 시간을 초기화합니다. 재연결을 기다리는 중이었다면 바로 재연결을 시도합니다.
* [재연결] 연결이 끊기면 이전 연결에서 받은 메시지와 `OnLeaveChannel` 콜백이 받은 순서대로 전달되도록 수정되었습니다. 재연결을 시작할 때까지 처리하지 못한 이전 연결의 메시지는 전달되지 않습니다.
* [중복 접속] `DUPLICATE_CONNECTION` 에러를 받으면 기존 연결을 종료하고 입장해 있던 채널마다 `OnLeaveChannel` 콜백을 전달하며, 자동으로 재연결하지 않습니다.
* [중복 접속] `DUPLICATE_CONNECTION` 에러로 연결이 종료된 뒤 SDK 함수를 호출하면 크래시가 발생할 수 있던 문제가 수정되었습니다.
* [연결 유지] 하트비트 전송 안정성이 개선되었습니다.
* [메시지] 채널 입장 시 전달되는 이전 메시지(`ChannelInfo.Messages`)에 `Language`, `Metadata`가 비어 있던 문제가 수정되었습니다.
* [메시지] 수신 메시지가 밀려 있으면 `Update()` 한 번에 여러 건을 처리하도록 개선되었습니다. `Update()`가 오랫동안 호출되지 않아 처리하지 못한 메시지가 1,000건 또는 8MB를 넘으면 연결을 다시 맺습니다.
* [안정성] 수신 메시지나 요청이 짧은 시간에 많이 쌓이면 크래시가 발생하거나 요청이 전송되지 않던 문제가 수정되었습니다.
* [신고] 서버에서 키워드 없이 전달된 신고 사유가 있을 때 `GetReportReasons()`의 사유가 하나의 키워드로 덮어써지던 문제가 수정되었습니다.
* [종료 처리] `Initialize` 직후 `Initialize` 또는 `Shutdown`을 다시 호출하면 크래시가 발생할 수 있던 문제가 수정되었습니다.
* [종료 처리] 콜백 함수 안에서 `Initialize` 또는 `Shutdown`을 호출하면 크래시가 발생할 수 있던 문제가 수정되었습니다.
* [연결] 재연결할 때마다 이전 연결의 스트림 자원이 남던 문제와, 서버가 데이터를 받지 않는 상태에서 `Shutdown()`이 반환되지 않을 수 있던 문제가 수정되었습니다.
* [iOS] iOS 26 이상에서 채널 입장·초기화 또는 재시작·재로그인 시 앱이 종료될 수 있던 문제가 수정되었습니다. SDK 작업 스레드를 `std::thread` 대신 POSIX 스레드로 생성하고, 포함된 iOS 라이브러리를 다시 빌드했습니다.
* [iOS] 포함된 iOS 라이브러리(gRPC 1.57.0, protobuf 4.23.4, Abseil 20230125.3, re2)를 같은 버전으로 Xcode 27.1에서 다시 빌드했습니다. 최적화 빌드이며 비트코드를 포함하지 않고, 최소 지원 버전은 iOS 15.0입니다.
* [빌드] UE 5.3 프로젝트를 C++20 또는 엄격한 표준 준수 모드(`/permissive-`, `BuildSettingsVersion.V4`)로 빌드할 때 SDK 헤더에서 컴파일 에러가 발생하던 문제가 수정되었습니다.
