---
title: Backend-5.18.14
date: 2026-08-27T10:00
slug: backend-5-18-14
---

:::info 업데이트 요약
[멀티 캐릭터] 하나의 계정에서 여러 캐릭터를 사용하는 멀티 캐릭터 기능이 추가되었습니다.  
[길드 기능] 길드 마스터가 장기 미접속일 때 길드원이 길드 마스터가 될 수 있는 길드 마스터 자동 교체 기능이 추가되었습니다.  
[길드 기능] 길드 조회 V4 함수가 추가되고, 기존 V3 조회 함수는 deprecated 되었습니다.  
[유저 관리] 토큰 유효성 체크가 계정·캐릭터 컨텍스트에서 모두 동작하도록 통합되었습니다.  
[멀티 캐릭터] 구 `Backend.MultiCharacter.Account` 함수가 삭제되었습니다. 해당 함수를 사용 중인 프로젝트는 `Backend.BMember`로 교체해야 합니다.  
[ToolKit] 구글 로그인 SDK(Android) 2.3.1이 배포되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.14/Backend-5.18.14.unitypackage" target="_blank">다운로드</a>   

## Versions
- Backend-5.18.14.dll
- Backend-1.1.0.aar

## 5.18.14 Update
**[Updates]**
- [멀티 캐릭터] 하나의 계정에서 여러 캐릭터를 사용하는 멀티 캐릭터 기능이 추가되었습니다.
   - 하나의 계정 아래 최대 20개의 캐릭터를 만들고, 선택한 캐릭터를 독립된 뒤끝 베이스 유저로 사용할 수 있습니다. [[멀티 캐릭터란]](/sdk-docs/backend/base/multi-character/what-is-multi-character)
   - `Backend.MultiCharacter.Character`의 `CreateCharacter`, `GetCharacterList`, `SelectCharacter`, `DeleteCharacter`로 캐릭터를 관리합니다. [[캐릭터 관리]](/sdk-docs/backend/base/multi-character/character/create-character)
   - `Backend.BMember.Elevate`로 기존 계정을 멀티 캐릭터 계정으로 전환할 수 있으며, `Backend.NeedsElevation`, `Backend.IsMultiAccountLogin`, `Backend.IsMultiCharacterLogin`으로 현재 컨텍스트를 구분합니다. [[계정 전환]](/sdk-docs/backend/base/multi-character/elevate)
   - 뒤끝 콘솔 > 개발 > 유저 > 환경설정에서 **멀티캐릭터 활성화** 버튼을 클릭하면 멀티 캐릭터를 활성화할 수 있습니다. 멀티 캐릭터로 변경한 프로젝트는 일반 프로젝트로 되돌릴 수 없습니다.
- [길드 기능] 길드 마스터 자동 교체 기능이 추가되었습니다.
   - 뒤끝 콘솔 > 뒤끝베이스 > 길드 관리 > 설정에 **길드 마스터 자동 교체 기준 일수**(1~365일)를 설정하면, 길드 마스터가 해당 일수를 초과해 접속하지 않은 길드에서 길드원이 `ClaimGuildMaster`를 호출해 길드 마스터가 될 수 있습니다. [[길드 마스터 자동 교체 신청]](/sdk-docs/backend/base/guild/guild-master/claim-master)
   - 길드 조회 결과에 길드 마스터의 마지막 로그인 시각(`masterLastLogin`)과 비활성 여부(`inactivedMaster`)가 추가되었습니다. 두 값은 기준 일수를 설정한 프로젝트의 V4 조회 함수에서만 내려옵니다.
   - 기준 일수를 설정하지 않은 프로젝트는 기존과 동일하게 동작하며, `ClaimGuildMaster` 호출은 statusCode 412로 실패합니다.
- [길드 기능] 길드 조회 V4 함수가 추가되었습니다.
   - `GetGuildListV4`, `GetMyGuildInfoV4`, `GetGuildInfoV4`가 추가되었습니다. [[모든 길드 리스트 조회]](/sdk-docs/backend/base/guild/search/get-all-guild)
   - V4 응답은 일반 JSON으로 정규화되어 내려오며, 단건 조회 응답의 최상위 키가 `guild`에서 `result`로 변경되었습니다. 반환값을 직접 파싱하는 경우 키 이름을 함께 수정해야 합니다.
   - `Backend.NewFunctions.Guild.GetAllGuildListV4`, `GetMyGuildInfoV4`, `GetGuildInfoV4`의 반환값은 `GetReturnValueByGuildList()`, `GetReturnValueByGuildItem()`으로 파싱 없이 사용할 수 있습니다.
- [유저 관리] 토큰 유효성 체크가 계정·캐릭터 컨텍스트에서 모두 동작하도록 통합되었습니다.
   - `IsAccessTokenAlive`가 멀티 캐릭터 계정 로그인 상태(캐릭터 미선택)에서도 정상적으로 토큰을 검증합니다. [[토큰 유효성 체크]](/sdk-docs/backend/base/user/token/verify)
   - 싱글 캐릭터 게이머의 성공 응답 statusCode가 204에서 **200**으로 변경되었습니다. 멀티 캐릭터의 계정·캐릭터 컨텍스트는 기존과 같이 **204**를 반환합니다. `GetStatusCode() == 204`로 성공을 판단하는 코드는 `IsSuccess()`를 사용하도록 수정해 주세요.
   - `Backend.BMember.VerifyToken`이 `IsAccessTokenAlive`와 동일하게 동작하는 함수로 추가되었습니다.
- [ToolKit] 구글 로그인 SDK(Android) 2.3.1이 배포되었습니다.
   - 2.3.0 unitypackage를 Unity 프로젝트에 import할 때 오류가 발생하던 문제를 수정하였습니다. [[구글 로그인 SDK 설치]](/sdk-docs/backend/toolkit/google-login/install-sdk)
   - 2.3.0 unitypackage를 다운로드한 경우에는 2.3.1 unitypackage를 새로 다운로드하여 사용해 주세요.

**[Deprecated]**
- [멀티 캐릭터] 구 멀티 캐릭터 계정 함수가 삭제되었습니다.
   - `Backend.MultiCharacter.Account`가 삭제되었습니다. 아래 함수를 사용하던 코드는 컴파일되지 않으므로 `Backend.BMember`의 대응 함수로 교체해야 합니다.
      - `CreateAccount`, `LoginAccount`, `AutoLoginAccount`, `LogoutAccount`, `WithdrawAccount`, `AuthorizeFederation`, `ChangeCustomToFederation`, `ChangeToFederation`, `DeleteLocalData`
      - `UpdateCustomEmail`, `ResetPassword`, `UpdatePassword`, `ConfirmCustomPassword`, `FindAccountId`
   - 멀티 캐릭터 계정의 회원가입·로그인·탈퇴는 싱글 캐릭터 프로젝트와 동일하게 `Backend.BMember`를 사용하며, 기존 계정을 멀티 캐릭터 계정으로 전환할 때만 `Backend.BMember.Elevate`를 호출합니다. [[계정 전환]](/sdk-docs/backend/base/multi-character/elevate)
   - 캐릭터 조회·생성·선택·삭제는 `Backend.MultiCharacter.Character`를 계속 사용합니다.
- [길드 기능] 길드 조회 V3 함수가 deprecated 되었습니다.
   - `GetGuildListV3`, `GetMyGuildInfoV3`, `GetGuildInfoV3` 호출 시 컴파일 경고가 발생합니다. 동작은 유지되므로 기존 코드는 계속 사용할 수 있으나, 신규 개발은 V4 함수를 사용해 주세요.

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
