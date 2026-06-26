---
title: Backend-5.18.0
date: 2025-06-18T10:00
slug: backend-5-18-0
---

:::danger 5.18.0 지원 종료
특정 상황에서 사용에 문제가 있는 에러가 발견되어 해당 버전의 지원이 종료되었습니다.  
- [게임 정보 관리] 트랜젝션 쓰기에서 `TransactionWriteV2` 함수가 특정 상황에서 에러가 발생하는 문제.  
- [유저 데이터 관리] 트랜젝션 쓰기에서 `TransactionWrite` 함수가 특정 상황에서 에러가 발생하는 문제.      

해당 이슈는 5.18.1 버전에서 수정이 되었으므로 5.18.1 버전을 사용해 주시기 바랍니다.
:::
:::info 업데이트 요약
[Android] Unity6에서 GameActivity로 진입점 설정시, 예외가 발생하던 문제를 수정했습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.18.0.dll
- Backend-1.1.0.aar

## 5.18.0 Update
**[Updates]**
- 뒤끝을 사용한 방치형 타이쿤 방식의 예제 게임이 추가 되었습니다.  [[신규 예제 게임]](/sdk-docs/backend/base/tutorial-game/example-game-idle-tycoon/example-idle-tycoon-game)

**[Fixes]**
- Unity6에서 GameActivity로 진입점 설정시, 예외가 발생하던 문제를 수정했습니다.  
- 실시간 알림에서 연결 종료로 자동 재접속이 발생할 때, 간헐적으로 재접속이 중복 발생하던 문제를 수정했습니다.  
- 리더보드 정보 조회시, isReset의 값을 항상 false로 전달하던 문제를 수정했습니다.  
- 로컬 차트를 최신 차트로 업데이트 할 때 뒤끝 콘솔에서 차트 파일이 적용되지 않은 차트를 로컬에서 삭제하도록 수정 했습니다.  
- `GetContentDictionarySortByChartName`함수가 Deprecated 되었습니다. `GetContentDictionarySortByChartId`함수를 사용해 주세요.  

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
