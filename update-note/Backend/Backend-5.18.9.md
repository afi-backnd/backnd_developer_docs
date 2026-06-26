---
title: Backend-5.18.9
date: 2026-04-14T10:00
slug: backend-5-18-9
---

:::danger 5.18.9 지원 종료
신규 버전 5.18.10가 릴리즈 됨에 따라 5.18.9 버전의 지원이 종료되었습니다.  
최신 버전으로 업데이트 해 주시기 바랍니다.
:::

:::info 업데이트 요약
[길드 기능] 길드 생성 시 오픈 길드 여부를 지정할 수 있는 옵션이 추가되었습니다.  
[그룹 기능] 그룹 변경 시 리더보드 연동 게임정보 처리 옵션이 추가되었습니다.  
[랜덤 조회] 그룹별 구분 동작이 추가되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.9/Backend-5.18.9.unitypackage" target="_blank">다운로드</a>   

## Versions
- Backend-5.18.9.dll
- Backend-1.1.0.aar

## 5.18.9 Update
**[Updates]**
- [길드 기능] 길드 생성 시 오픈 길드 여부를 지정할 수 있는 옵션이 추가되었습니다.
   - `CreateGuildV4`에 `isOpenGuild` 파라미터가 추가되어 오픈 길드 여부를 함께 설정할 수 있습니다.
- [그룹 기능] 그룹 변경 시 리더보드 연동 게임정보 처리 옵션이 추가되었습니다.
   - 그룹 변경 시 이전 그룹의 리더보드에 등록된 데이터는 삭제되며, `Update` 함수의 `LeaderboardValuePolicy` 파라미터로 리더보드에 사용되는 집계 필드 데이터를 유지/초기화/리더보드 재등록할지 선택할 수 있습니다.
- [랜덤 조회] 그룹별 구분 동작이 추가되었습니다.
   - 콘솔 설정에 따라 랜덤 조회를 그룹 기준으로 운영할 수 있으며, 그룹 변경 시 RandomPool 데이터도 변경된 그룹의 RandomPool로 이동합니다.

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
