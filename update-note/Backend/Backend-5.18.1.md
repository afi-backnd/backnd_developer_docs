---
title: Backend-5.18.1
date: 2025-08-08T10:00
slug: backend-5-18-1
---

:::danger 5.18.1 지원 종료
일부 함수 사용 시 특정 상황에서 에러가 발견되어 5.18.1 버전의 지원이 종료되었습니다.  
해당 이슈는 5.18.12 버전에서 수정이 되었으므로 5.18.2 버전을 사용해 주시기 바랍니다.
:::
:::info 업데이트 요약
[게임 정보 관리] 트랜젝션 쓰기에서 `TransactionWriteV2` 함수가 특정 상황에서 에러가 발생하는 현상을 수정하였습니다.  
[유저 데이터 관리] 트랜젝션 쓰기에서 `TransactionWrite` 함수가 특정 상황에서 에러가 발생하는 현상을 수정하였습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.18.1.dll
- Backend-1.1.0.aar

## 5.18.1 Update
**[Fixes]**
- 게임 정보 관리 트랜젝션 쓰기에서 `TransactionWriteV2` 함수를 동기로 사용했을때 데이터가 정상적으로 처리 되지 않는 현상을 수정하였습니다.  
- 유저 데이터 관리 트랜젝션 쓰기에서 `TransactionWrite` 함수를 동기로 사용했을때 데이터가 정상적으로 처리 되지 않는 현상을 수정하였습니다.  

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
