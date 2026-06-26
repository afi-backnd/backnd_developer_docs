---
title: Backend-5.11.5
date: 2024-01-30T10:00
slug: backend-5-11-5
---

:::info 업데이트 요약
원스토어 푸시기능 및 그외 기능이 추가 및 수정되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.11.5/Backend-5.11.5.unitypackage" target="_blank">다운로드</a>   


## Versions
- Backend-5.11.5.dll
- Backend-1.0.9.aar

## 5.11.5 Update
**[Updated]**

- [푸시 관리] 안드로이드 환경에서 구글 플레이 스토어용 푸시와 원스토어용 푸시를 보낼 수 있도록 푸시 기능이 변경되었습니다.
- [유저 정보관리] 멀티 캐릭터 페데레이션 전환 기능이 추가되었습니다.
- [ToolKit] 구글 로그인 SDK에 구글 로그아웃 기능이 추가되었습니다.
- [ToolKit] 구글 로그인 SDK의 네임스페이스가 변경되었습니다.
- [실시간 알림] 실시간 알림 접속이 끊어질 경우, 1초 간격으로 재접속을 시도하는 로직이 추가되었습니다.

**[Fixed]**

- [게임 정보관리] 스키마 테이블에서 데이터를 null로 수정할 경우, NULL: true로 데이터가 수정되는 에러를 픽스했습니다.  
param.Add("data", null)과 같은 데이터가 요청될 경우, 'bad {data} dataType, 잘못된 {data} dataType 입니다' 에러가 발생합니다.
- [게임 정보관리] 스키마 테이블에서 DateTime 형식의 데이터를 수정할 경우, 설정된 형식이 아닌 요청한 값 형태 그대로 적용되는 에러를 픽스했습니다.  
설정한 DateTime의 형식과 동일한 DateTime을 요청해야만 데이터가 등록됩니다.  
형식이 일치하지 않을 경우, 'bad time dataType, 잘못된 time dataType 입니다' 에러가 발생합니다.  
형식이 YYYY-MM-DD일 경우, param.Add("startDate", "2024-01-30")과 같이 데이터를 등록해야 합니다.
- [게임 정보관리] 스키마 테이블에서 시간만 존재(HH:mm:ss)하는 데이터 타입 수정 시, Invalid Date로 저장되는 에러를 픽스했습니다.
- [유저 데이터 관리] 스키마 테이블에서 null 비허용에 기본값이 0인 경우 무조건 '{columnName} is not null option' 에러가 발생하는 문제를 픽스했습니다.
- [유저 데이터 관리] 스키마 테이블에서 null 비허용인 컬럼이 있을 경우, 데이터 삽입 시 해당 컬럼이 param에 존재하지 않으면 콘솔에서 설정한 기본값으로 자동 삽입이 안되던 에러를 픽스했습니다.
- [게임 정보관리] 트랜잭션 쓰기 작업 도중 transactionList가 변경될 경우, 트랜잭션 쓰기에 대한 요청이 오지 않는 에러를 픽스했습니다.
- [유저 정보관리] 멀티 캐릭터에서 만료된 액세스토큰으로 자동 로그인 시, 비정상적인 아이디가 새로 생성되는 에러를 픽스했습니다.
- [유저 정보관리] 커스텀 로그인에서 스팀 페데레이션 전환, 안드로이드에서 애플 로그인 페데레이션 전환 시, 에러가 발생하던 문제를 픽스했습니다.
- [국가정보] 초기화 이후 현재 국가, 지역, 언어를 불러오는 로직의 비동기 방식에 대한 예외 처리를 보완했습니다.

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| EngineIoClientDotNet 0.9.22   | 0.9.22     | The MIT License                     |
| SocketIoClientDotNet 0.9.13   | 0.9.13     | The MIT License                     |
| ILRepack.Lib.MSBuild.Task     | 2.0.18.1   | The MIT License                     |
| Newtonsoft.Json 12.0.3        | 12.0.3     | The MIT License                     |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.16.0     | The Unlicense                       |
| .NET Reactor                  | 6.9.0.0    | End-User License Agreement("EULA") |
