---
title: Chat-1.4.4
date: 2026-10-20T10:00
slug: chat-1-4-4
---

:::info 업데이트 요약
온디맨드 번역 결과를 번역 도착 언어별로 확인할 수 있도록 `OnTranslateMessage` 콜백이 변경되었습니다.  
유저 간 차단 에러 전달과 일부 함수의 매개변수 검증 처리가 수정되었습니다.  
재연결 후 채널에 다시 입장하지 않거나 콜백 순서가 뒤바뀌던 문제가 수정되었습니다.
:::

:::danger 함수 변경 안내
`IChatClientListener.OnTranslateMessage`의 매개변수가 `List<MessageInfo>`에서 `Dictionary<string, MessageInfo>`로 변경되었습니다.  
기존 코드를 그대로 사용하는 경우 컴파일 에러가 발생하니, [마이그레이션 가이드](/sdk-docs/chat/install/migration-guide)를 참고하여 수정해 주세요.
:::

<!--truncate-->

:::info 뒤끝베이스 SDK 5.11.9 이상 안내
BackndChat-1.4.4 이상을 사용하실 경우에는 Backend SDK 버전이 5.11.9 이상이어야 합니다.  
채팅 기능만 사용할 경우에는 아래 SDK만 import해 주시기 바랍니다. (뒤끝베이스는 로그인 기능 외 사용 불가)
:::

[Unity SDK] <a href="https://developer.thebackend.io/sdk/chat/1.4.4/BackndChat-1.4.4.unitypackage" id="download-sdk-chat">다운로드</a>  
[Unreal 5.3 SDK] <a href="https://developer.thebackend.io/sdk/chat/1.4.4/BackndChat-1.4.4-UE53.zip" id="download-sdk-chat">다운로드</a>  
[Unreal 4.27 SDK] <a href="https://developer.thebackend.io/sdk/chat/1.4.4/BackndChat-1.4.4-UE427.zip" id="download-sdk-chat">다운로드</a>  


## Versions
- BackndChat-1.4.4.dll

## 1.4.4 Update
* [번역] `OnTranslateMessage` 콜백의 매개변수가 `Dictionary<string, MessageInfo>`로 변경되었습니다. 키는 요청한 번역 도착 언어 코드이며, 번역에 실패한 언어는 포함되지 않습니다. (언어별 번역 결과를 제공하지 않는 채팅 서버에서는 키가 `#0`, `#1`... 형식으로 전달됩니다.)
* [유저 간 차단] 서버에서 전달한 `ALREADY_BLOCK_PLAYER`, `NOT_FOUND_BLOCK_URL` 에러가 `UNKNOWN_ERROR`로 전달되던 문제가 수정되었습니다.
* [매개변수 검증] `SendWhisperMessage`, `SendReportChatMessage`, `SendTranslateChatMessage`, `SendAddBlockGamer`, `SendRemoveBlockGamer`에 잘못된 값을 넣으면 `INVALID_PARAMETER` 에러를 전달한 뒤에도 처리가 계속되던 문제가 수정되었습니다. 이제 요청을 보내지 않고 `INVALID_PARAMETER` 에러만 전달합니다.
* [재연결] 연결이 끊긴 직후 이전 연결의 응답이 처리되면 재연결 후 직접 입장했던 채널에 다시 입장하지 않던 문제가 수정되었습니다.
* [재연결] 연결이 끊기면 이전 연결에서 받은 메시지와 `OnLeaveChannel` 콜백이 받은 순서대로 전달되도록 수정되었습니다.
* [재연결] `Update()`를 오랫동안 호출하지 않다가 다시 호출하면 콜백 순서가 뒤바뀌거나 자동 입장 채널이 직접 입장한 채널로 처리될 수 있던 문제가 수정되었습니다.
