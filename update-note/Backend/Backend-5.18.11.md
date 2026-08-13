---
title: Backend-5.18.11
date: 2026-06-10T10:00
slug: backend-5-18-11
---

:::danger 5.18.11 지원 종료
신규 버전 5.18.12가 릴리즈 됨에 따라 5.18.11 버전의 지원이 종료되었습니다.  
최신 버전으로 업데이트 해 주시기 바랍니다.
:::

:::info 업데이트 요약
[버전 관리] 버전 관리에 스팀이 추가 되었습니다.  
[데이터 처리] Param.Parse 함수 오류가 수정 되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.11/Backend-5.18.11.unitypackage" target="_blank">다운로드</a>   

## Versions
- Backend-5.18.11.dll
- Backend-1.1.0.aar

## 5.18.11 Update
**[Updates]**
- [버전 관리] 버전 관리에 스팀이 추가 되었습니다.
   - `GetLatestVersion` 함수의 `VersionInfo`를 통해 스팀을 선택할 수 있습니다.

**[Fixes]**
- [데이터 처리] Param.Parse 함수 오류가 수정 되었습니다.
   - 수십 KB 크기의 JSON 문자열을 `Param.Parse(string)`로 파싱할 때 간헐적으로 IndexOutOfRangeException이 발생하는 문제를 수정 하였습니다.

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
