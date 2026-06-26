---
title: Backend-5.14.0
date: 2024-09-24T10:00
slug: backend-5-14-0
---

:::danger 5.14.0 지원 종료
특정 상황에서 이용에 불편을 주는 에러가 발견되어 해당 버전의 지원이 종료되었습니다.  
- [국가 기능] ios에서 location 정보를 읽어오지 못하던 문제를 수정했습니다.

해당 이슈는 5.14.1 버전에서 수정이 되었으므로 5.14.1 버전을 사용해 주시기 바랍니다.
:::

:::info 업데이트 요약
그룹 기능이 추가되었습니다.  
리더보드 기능이 추가되었습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.14.0.dll
- Backend-1.0.9.aar

## 5.14.0 Update

**[Updated]**  
- 그룹 기능이 추가되었습니다.
- 리더보드 기능이 추가되었습니다.
  -  그룹 구분된 리더보드를 사용할 경우, URank 대신 Leaderboard 함수를 호출해야 합니다.

**[Fix]**
- [게임정보관리] Insert, Update, 트랜잭션 함수의 콜백에서 Exception 발생 시 콜백이 2번 호출되는 에러가 수정되었습니다. (5.12.0 ~ 5.13.0 버전에서 발생)
- [게임정보관리] ios 16 이상의 기기에서 select 절을 이용한 Get 함수 호출 시, Malformed URL 에러가 발생하는 현상을 수정하였습니다. (5.12.0 ~ 5.13.0 버전에서 발생)
- [게임정보관리] PlayerData의 트랜잭션 호출 시, GetWriteCapacity, GetReadCapacity를 호출할 경우 Exception이 발생하는 에러가 수정되었습니다. (5.12.0 ~ 5.13.0 버전에서 발생)
- [유저 관리] iOS에서 CheckUserInBackend 호출 시 bad signature 발생하는 에러가 수정되었습니다. (5.12.0 ~ 5.13.0 버전에서 발생)
- [뒤끝 펑션] 윈도우로 빌드된 EXE 파일에서 뒤끝 펑션 호출 시, 펑션에서 bad app id 에러가 발생하던 에러가 수정되었습니다.
- [최적화] 함수 호출 후 간혈적으로 memory leak 에러 메세지가 발생하던 문제가 수정되었습니다. (5.12.0 ~ 5.13.0 버전에서 발생)
- [ToolKit] Backend 초기화 이전에 SendQueue 사용시, Null 에러가 발생하던 문제가 수정되었습니다. (5.12.0 ~ 5.13.0 버전에서 발생)
## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
