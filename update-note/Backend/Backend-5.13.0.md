---
title: Backend-5.13.0
date: 2024-08-27T10:00
slug: backend-5-13-0
---

:::danger 5.13.0 지원 종료
특정 상황에서 이용에 불편을 주는 에러가 발견되어 해당 버전의 지원이 종료되었습니다.  
- [게임정보관리] Insert, Update, 트랜잭션 함수의 콜백에서 Exception 발생 시 콜백이 2번 호출되는 에러
- [게임정보관리] ios 16 이상의 기기에서 select 절을 이용한 Get 함수 호출 시, Malformed URL 에러
- [게임정보관리] 트랜잭션에 대한, GetWriteCapacity, GetReadCapacity 에러
- [유저 관리] iOS에서 CheckUserInBackend 호출 시 에러

해당 이슈는 5.14.0 버전에서 수정이 되었으므로 5.14.0 버전을 사용해 주시기 바랍니다.
:::

:::info 업데이트 요약
차트, 확률 파일 다운로드 기능이 추가되었습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.13.0.dll
- Backend-1.0.9.aar

## 5.13.0 Update
**[Updated]**  
- 성능이 대폭 개선된 차트, 확률 파일 다운로드 기능이 추가되었습니다.  
- 영수증 검증에 가격과 통화를 추가할 수 있게 되었습니다.

**[Fix]**  
- [길드] 5.12.0, 5.12.1 버전에서 GetGuildMemberListV3, GetGuildListV3의 limit 기본값이 10으로 적용되던 오류를 수정했습니다.
- [유저 관리] ChangeCustomToFederation 함수 사용 시 bad signature 에러가 발생하는 오류를 수정했습니다.
- [뒤끝 매치] OnMatchInGameAccess 핸들러의 GameRecord가 null인 오류를 수정했습니다.
## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
