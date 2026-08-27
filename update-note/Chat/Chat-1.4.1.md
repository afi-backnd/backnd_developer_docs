---
title: Chat-1.4.1
date: 2026-08-27T10:00
slug: chat-1-4-1
---

:::info 업데이트 요약
재연결 안정성이 개선되었습니다.  
씬 로딩이나 앱 일시정지에서 복귀할 때 즉시 재연결을 시도하며, 재연결 이후 이전 연결에서 수신된 메시지가 콜백으로 전달되지 않도록 수정되었습니다.
:::

<!--truncate-->

:::info 뒤끝베이스 SDK 5.11.9 이상 안내
BackndChat-1.4.1 이상을 사용하실 경우에는 Backend SDK 버전이 5.11.9 이상이어야 합니다.  
채팅 기능만 사용할 경우에는 아래 SDK만 import해 주시기 바랍니다. (뒤끝베이스는 로그인 기능 외 사용 불가)
:::

[Unity SDK] <a href="https://developer.thebackend.io/sdk/chat/1.4.1/BackndChat-1.4.1.unitypackage" id="download-sdk-chat">다운로드</a>  
[Unreal 5.3 SDK] <a href="https://developer.thebackend.io/sdk/chat/1.3.0/BackndChat-1.3.0-UE53.zip" id="download-sdk-chat">다운로드</a>  
[Unreal 4.27 SDK] <a href="https://developer.thebackend.io/sdk/chat/1.3.0/BackndChat-1.3.0-UE427.zip" id="download-sdk-chat">다운로드</a>  


## Versions
- BackndChat-1.4.1.dll

## 1.4.1 Update
* [재연결] 씬 로딩이나 앱 일시정지에서 복귀하면 재연결 대기 시간을 초기화하고 즉시 재연결을 시도하도록 개선되었습니다.
* [재연결] 재연결 이후 이전 연결에서 수신된 메시지가 콜백으로 전달되던 문제가 수정되었습니다.
* [재연결] 네트워크 처리가 Unity 메인 루프와 분리되어 씬 전환 및 일시정지 중에도 연결 처리가 유지됩니다.
* [연결 유지] 하트비트 전송 안정성이 개선되었습니다.
* [인증] 뒤끝베이스 로그인 정보를 확인할 수 없는 상태가 지속되면 OnError 콜백으로 NOT_AUTHENTICATION 에러를 전달합니다.
* [종료 처리] Dispose 호출 시점의 안정성이 개선되었습니다.
