---
title: Backend-5.17.0
date: 2025-04-28T10:00
slug: backend-5-17-0
---

:::danger 5.17.0 지원 종료
특정 상황에서 사용에 문제가 있는 에러가 발견되어 해당 버전의 지원이 종료되었습니다.  
- [클라우드 세이브] 특정 데이터 형식이 클라우드 세이브로 업로드 되지 않던 문제.     

해당 이슈는 5.17.1 버전에서 수정이 되었으므로 5.17.1 버전을 사용해 주시기 바랍니다.
:::

:::info 업데이트 요약
[계정 전환] 페더레이션 계정 간 로그인 방식 전환 기능 추가했습니다.    
[유저 정보] 유저 정보 조회 API의 V2버전을 추가했습니다.  
[구글 인증] 신규 구글 계정 인증 SDK를 추가했습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.17.0.dll
- Backend-1.0.9.aar

## 5.17.0 Update
**[Updates]**
- 페더레이션 계정 간 로그인 전환 기능을 추가했습니다. [[페더레이션 전환 문서]](/sdk-docs/backend/base/user/federation/migrate-to-federation)  
- 멀티 캐릭터의 계정 전환에서 ChangeCustomToFederation 함수가 사용 중단(Deprecated)되었습니다.  
  - 기존 기능을 포함한 ChangeToFederation 함수를 추가했습니다. [[멀티 캐릭터 문서]](/sdk-docs/backend/base/user/multi-character/account/change-federation-to-federation)  
- 유저 정보 조회 API의 V2버전을 추가했습니다.  
  - 해당 유저가 속한 Group 이름이 리턴값에 포함되어 있습니다.  
  - GetUserInfoV2  
  - GetUserInfoByInDateV2
  - GetUserInfoByNickNameV2
- 신규 구글 계정 인증 SDK를 추가했습니다. [[구글로그인 SDK]](/sdk-docs/backend/toolkit/google-login/install-sdk)  
  - 안드로이드 인증관리자를 사용하는 신규 구글 계정 인증을 제공합니다.  


**[Fixes]**
- 뽑기 확률 테이블에서 ContentJson을 사용해서 Json 형식으로 변환시 오버플로우가 발생하는 현상을 수정했습니다.  

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
