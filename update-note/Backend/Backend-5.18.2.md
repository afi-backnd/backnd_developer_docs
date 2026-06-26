---
title: Backend-5.18.2
date: 2025-09-10T10:00
slug: backend-5-18-2
---

:::danger 5.18.2 지원 종료
일부 함수 사용 시 특정 상황에서 에러가 발견되어 5.18.12 버전의 지원이 종료되었습니다.  
해당 이슈는 5.18.3 버전에서 수정이 되었으므로 5.18.3 버전을 사용해 주시기 바랍니다.
:::

:::info 업데이트 요약
[리더보드 기능] 리더보드 불러오기에서 `LeaderboardTableItem` 에 누락된 변수를 수정 하였습니다.  
[페더레이션 인증] `GetGPGS2AccessToken` 함수 에러 처리 로직을 수정하였습니다.  
[트랜젝션] 유저데이터/게임정보 `TransactionWrite` 사용시 특정 상황에서 발생하는 에러를 수정하였습니다.  
[길드 기능] `GetGuildInDateByGuildName` 로 검색할 수 있는 범위가 개선 되었습니다.  
[길드 기능] 길드굿즈 기부/사용, 길드 메타데이터 수정 함수의 기능이 개선 된 함수가 추가 되었습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.18.2.dll
- Backend-1.1.0.aar

## 5.18.2 Update
**[Updates]**
- [길드 기능] `GetGuildInDateByGuildName` 로 검색할 수 있는 범위가 개선 되었습니다. [[길드 정보 조회]](/sdk-docs/backend/base/guild/search/get-guild-by-indate)  
  - 파라미터를 추가하여 플레이어가 속해있는 그룹에 상관없이 전체 그룹에서 길드를 검색 할 수 있습니다.  
- [길드 기능] 길드굿즈 기부/사용, 길드 메타데이터 수정 함수의 기능이 개선 된 함수가 추가 되었습니다.  
  - `ContributeGoodsV5`, `UseGoodsV5`, `ModifyGuildV5` 함수가 추가 되었습니다.   
  - 추가된 함수들은 리더보드 초기화 시간을 제외한 시간에 정상 동작합니다.
  - `ContributeGoodsV5` 함수가 추가됨에 따라 기존 `ContributeGoodsV3`, `ContributeGoodsV4`는 Deprecated 되었습니다. [[굿즈 기부]](/sdk-docs/backend/base/guild/goods/donate-forbidden-during-reset-time)    
  - `UseGoodsV5` 함수가 추가됨에 따라 기존 `UseGoodsV3`, `UseGoodsV4`는 Deprecated 되었습니다.  
    [[굿즈 사용]](/sdk-docs/backend/base/guild/guild-master/use-goods-forbidden-during-reset-time)    
  - `ModifyGuildV5` 함수가 추가됨에 따라 기존 `ModifyGuildV3`, `ModifyGuildV3`는 Deprecated 되었습니다.  [[길드 메타 데이터 변경]](/sdk-docs/backend/base/guild/metadata/change-forbidden-during-reset-time)    

**[Fixes]**
- [리더보드 기능] 리더보드 불러오기에서 `LeaderboardTableItem` 에 누락된 변수를 수정 하였습니다.  
  - `LeaderboardTableItem`으로 데이터를 가져올때 리더보드 초기화 시간(initializationTime)도 가져올수있도록 수정하였습니다.  
- [페더레이션 인증] `GetGPGS2AccessToken` 함수 에러 처리 로직을 수정하였습니다.  
  - 특정 에러 발생시 콜백이 중복으로 처리되는 구조를 수정 하였습니다.  
  - 특정 에러로 발생하는 에러 메시지를 수정하였습니다.  
- [트랜젝션] 유저데이터/게임정보 `TransactionWrite` 사용시 특정 상황에서 발생하는 에러를 수정하였습니다.
  - `TransactionWrite` 함수 사용시 테이블 데이터가 압축데이터 일 때 Param이 없는 함수를 사용하면 발생하는 에러를 수정하였습니다.

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
