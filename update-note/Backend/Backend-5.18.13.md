---
title: Backend-5.18.13
date: 2026-08-13T10:00
slug: backend-5-18-13
---

:::danger 5.18.13 지원 종료
5.18.13 및 5.18.14에서 숫자로만 구성된 문자열 키가 포함된 Dictionary를 일부 요청에 전달할 경우 401 `bad signature` 오류가 발생할 수 있습니다.<br />
문제가 수정된 5.18.15로 업데이트해 주세요.
:::

:::info 업데이트 요약
[길드 기능] 길드 굿즈 기부/사용 함수에 double 타입 amount가 추가되었습니다.  
[페더레이션 인증] 스팀 로그인에 Partner(Web API) 방식 인증이 추가되었습니다.  
[영수증 검증] details의 한글이 유니코드 이스케이프 형태로 저장되는 문제가 수정되었습니다.  
[위치 정보] 자동 갱신 결과가 수동 설정 값을 덮어쓰는 문제가 수정되었습니다.  
[ToolKit] 구글 로그인 SDK(Android) 2.3.0이 배포되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.13/Backend-5.18.13.unitypackage" target="_blank">다운로드</a>   

## Versions
- Backend-5.18.13.dll
- Backend-1.1.0.aar

## 5.18.13 Update
**[Updates]**
- [길드 기능] 길드 굿즈 기부/사용 함수에 double 타입 amount가 추가되었습니다.
   - `ContributeGoodsV5`, `UseGoodsV5` 함수에 double 타입 `amount` 오버로드가 추가되어 int 범위를 초과하는 수치를 전달할 수 있습니다.
- [페더레이션 인증] 스팀 로그인에 Partner(Web API) 방식 인증이 추가되었습니다.
   - 뒤끝 콘솔 > 프로젝트 설정 > 인증 정보 > 스팀 계정 인증 정보에 Partner Identity를 입력한 후, `SteamUser.GetAuthTicketForWebApi(identity)`로 발급한 티켓으로 기존과 동일하게 페더레이션 로그인을 진행하면 서버가 Partner API로 검증합니다. [[Steam 로그인 인증 예제]](/sdk-docs/backend/base/user/federation/example-using-steam)
   - Partner Identity를 입력하지 않으면 기존 방식이 그대로 동작하며, Partner Identity를 입력한 상태에서 기존 방식으로 로그인하면 오류가 발생합니다.
- [데이터 처리] 2^53을 초과하는 정수 파라미터에 대한 경고 로그가 추가되었습니다.
   - `Param`에 2^53을 초과하는 정수를 추가할 경우 서버 저장 시 정밀도가 손실될 수 있어 경고 로그가 출력됩니다. 해당 값은 문자열로 전송해 주세요.
- [ToolKit] 구글 로그인 SDK(Android) 2.3.0이 배포되었습니다.
   - 로그인 도중 화면 회전·프로세스 재시작 시 앱이 강제 종료되거나 콜백이 유실되던 문제, 콜백이 중복 호출될 수 있던 문제가 수정되었습니다. [[구글 로그인 SDK 설치]](/sdk-docs/backend/toolkit/google-login/install-sdk)

**[Fixes]**
- [길드 기능] 큰 재화 수치 전송 시 값이 유실되는 문제가 수정되었습니다.
   - 길드 굿즈 기부/사용 및 길드 재화 리더보드 갱신 시 매우 큰 수치가 지수 표기(예: 3E+21)로 전송되어 값이 유실될 수 있는 문제를 수정하였습니다.
- [영수증 검증] details의 한글이 유니코드 이스케이프 형태로 저장되는 문제가 수정되었습니다.
   - 영수증 검증/환불 요청 시 `details` 등 요청 데이터에 포함된 한글이 `\uXXXX` 형태로 저장되는 문제를 수정하였습니다.
- [위치 정보] 자동 갱신 결과가 수동 설정 값을 덮어쓰는 문제가 수정되었습니다.
   - 위치 정보 자동 조회(autoLoadLocationProperties)와 수동 설정(`CustomizeLocationProperties`)을 함께 사용할 때, 나중에 완료된 자동 조회 결과가 수동 설정 값을 덮어쓰지 않도록 수정하였습니다.

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
