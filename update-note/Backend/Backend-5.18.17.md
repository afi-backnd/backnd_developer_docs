---
title: Backend-5.18.17
date: 2026-09-10T10:00
slug: backend-5-18-17
---

:::info 업데이트 요약
문화권 관련 오류가 수정되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.17/Backend-5.18.17.unitypackage" target="_blank">다운로드</a>

## Versions
- Backend-5.18.17.dll
- Backend-1.1.0.aar

## 5.18.17 Update
**[Fixes]**
- 문화권 관련 오류 수정
   - 터키어 등 일부 문화권에서 iOS 플랫폼을 잘못 판별해 인증 요청의 플랫폼 정보(`os`)가 잘못 전달되거나 앱 식별자(`app_id`)가 누락되던 문제를 수정하였습니다.
   - 커스텀·게스트 회원가입/로그인, 페더레이션 로그인·가입 여부 확인·계정 전환, 토큰 로그인·갱신, 멀티 캐릭터 계정 전환(`Elevate`) 및 캐릭터 생성·선택·삭제 요청에 수정이 적용됩니다. `ChangeCustomToFederation` 호출 시 발생하던 `bad appid` 오류도 이에 해당합니다. [[커스텀 → 페더레이션 계정 전환]](/sdk-docs/backend/base/user/federation/migrate-from-custom)
   - 같은 플랫폼 판별 문제로 발생하던 통합 영수증 검증 실패와 인자 없는 `GetLatestVersion()` 호출 오류도 함께 수정되었습니다.
   - 게임 로그 저장 시 `item`과 `ITEM`처럼 대소문자만 다른 Param 키의 중복 검사 결과가 문화권에 따라 달라지던 문제를 수정하였습니다. [[게임 로그 저장]](/sdk-docs/backend/base/game-log/insert)

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
