---
title: Chat-1.2.0
date: 2024-06-25T09:00
slug: chat-1-2-0
---

:::danger 1.2.0 지원 종료
해당 버전은 더 이상 지원되지 않습니다.
:::

:::info 업데이트 요약
[Unity] 커스텀 인증 기능, 번역 기능이 추가되었습니다. namespace가 BackndChat으로 변경되었습니다.  
[Unreal] 언리얼 SDK가 추가되었습니다.  
[Platform API] Platform API가 추가되었습니다.
:::

:::danger 함수 변경 안내  
채팅 SDK의 모든 함수가 Backend에서 Backnd로 변경되었습니다.  
기존 함수를 그대로 사용하는 경우 에러가 발생하게 되니, 반드시 변경된 함수를 꼭 확인해 주세요.  
:::  

:::warning CHAT Unity SDK 1.2.1 버전 이용 안내
뒤끝 베이스 SDK의 액세스 토큰이 재발급되었을 경우, 채팅 서버의 재접속이 실패하는 현상이 확인되어, 핫픽스가 진행되었습니다.  
해당 오류의 수정 버전인 1.2.1 버전을 이용해 주시기 바랍니다.
:::

<!--truncate-->

[Unity SDK] <s>다운로드</s>  
[Unreal 5.3 SDK] <a href="https://developer.thebackend.io/sdk/chat/1.2.0/BackndChat-1.2.0-UE53.zip" id="download-sdk-chat">다운로드</a>  
[Unreal 4.27 SDK] <a href="https://developer.thebackend.io/sdk/chat/1.2.0/BackndChat-1.2.0-UE427.zip" id="download-sdk-chat">다운로드</a>  

## Versions
- BackndChat-1.2.0.dll

## 1.2.0 Update
* **채팅 SDK의 모든 함수가 Backend에서 Backnd로 변경되었습니다.**
* 뒤끝 Base SDK 외의 서버와 연동가능한 커스텀 인증 기능이 추가되었습니다.
* 채팅 번역 기능이 추가되었습니다.
* 자동 확장 로직이 개선되었습니다.
* 언리얼 SDK가 추가되었습니다.
* 채팅 관련 API를 호출할 수 있는 Platform API 기능이 추가되었습니다.
