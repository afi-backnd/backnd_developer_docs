---
title: Backend-5.18.10
date: 2026-04-29T18:00
slug: backend-5-18-10
---

:::danger 5.18.10 지원 종료
신규 버전 5.18.11가 릴리즈 됨에 따라 5.18.10 버전의 지원이 종료되었습니다.  
최신 버전으로 업데이트 해 주시기 바랍니다.
:::

:::info 업데이트 요약
[페더레이션] GPGS V2 로그인 오류가 수정 되었습니다.  
[공지사항] 비동기 NoticeListV2 offset 함수 오류가 수정 되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.10/Backend-5.18.10.unitypackage" target="_blank">다운로드</a>   

## Versions
- Backend-5.18.10.dll
- Backend-1.1.0.aar

## 5.18.9 Update
**[Fixes]**
- [페더레이션] GPGS V2 로그인 오류가 수정 되었습니다.
   - GPGS V2 로그인이 정상적으로 되지 않는 문제를 수정 하였습니다.
- [공지사항] 비동기 NoticeListV2 offset 함수 오류가 수정 되었습니다.
   - 비동기 NoticeListV2 함수 중에 offset을 입력 하는 함수 호출 시 제대로 다음 공지사항 리스트를 가져 오지 못하는 오류를 수정 하였습니다.

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
