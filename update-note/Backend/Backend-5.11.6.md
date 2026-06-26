---
title: Backend-5.11.6
date: 2024-02-27T10:00
slug: backend-5-11-6
---

:::info 업데이트 요약
GPGS V2 로그인, 확률관리 등 여러 기능이 추가 및 수정되었습니다.
:::
<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.11.6/Backend-5.11.6.unitypackage" target="_blank">다운로드</a>   


## Versions
- Backend-5.11.6.dll
- Backend-1.0.9.aar

## 5.11.6 Update
**[Updated]**

- [게임 유저 관리] GPGS V2 로그인 기능이 추가되었습니다.  
- [확률 관리] 확률에 등록된 차트의 정보를 불러오는 기능이 추가되었습니다.(확률에 등록된 아이템의 확률들을 확인할 수 있습니다.)

**[Fixed]**

- [애플 로그인 SDK] 애플 로그인 웹뷰가 활성화된 상태로 백그라운드에 진입했다 돌아올 경우, 웹뷰가 사라지는 현상을 수정했습니다.  

**[Server]**
- [공통 Error Cases] 클라이언트와 서버 시간이 10분 이상 차이가 발생할 경우, 현재 클라이언트 시간이 서버 시간보다 빠르면 bad client_date 에러가 발생하지 않던 문제가 수정되었습니다.  
- [길드 기능] 내 길드 굿즈 불러오기(Backend.Guild.GetMyGuildGoodsV3) 기능의 성능이 개선되었습니다.  
- [우편 관리] 관리자 우편 불러오기 기능의 성능이 개선되었습니다.  

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
