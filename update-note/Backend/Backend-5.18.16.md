---
title: Backend-5.18.16
date: 2026-09-04T10:00
slug: backend-5-18-16
---

:::info 업데이트 요약
[압축 데이터] 압축 테이블에서 페이징 키를 사용한 다음 페이지 조회가 정상적으로 동작하지 않던 문제가 수정되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.16/Backend-5.18.16.unitypackage" target="_blank">다운로드</a>   

## Versions
- Backend-5.18.16.dll
- Backend-1.1.0.aar

## 5.18.16 Update
**[Fixes]**
- [압축 데이터] 압축 테이블에서 페이징 키를 사용한 다음 페이지 조회가 정상적으로 동작하지 않던 문제가 수정되었습니다.
   - 압축 데이터가 설정된 게임 정보 또는 유저 데이터 테이블에서 `firstKey` 또는 `LastEvaluatedKey`를 사용한 다음 페이지 조회가 정상적으로 동작하도록 수정하였습니다.
   - `firstKey`는 `FirstKeystring()`, `LastEvaluatedKey`는 `LastEvaluatedKeyString()`으로 가져와 다음 페이지 조회에 사용할 수 있습니다. [[게임 정보 압축데이터 주의사항]](/sdk-docs/backend/base/game-information/compress-caution) [[유저 데이터 압축데이터 주의사항]](/sdk-docs/backend/base/player-data/compress-caution)

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
