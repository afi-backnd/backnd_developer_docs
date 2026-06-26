---
title: Backend-5.11.8
date: 2024-05-28T10:00
slug: backend-5-11-8
---

:::info 업데이트 요약
PlayerData 스키마 테이블 기본값 삽입 이슈가 해결되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.11.8/Backend-5.11.8.unitypackage" target="_blank">다운로드</a>   


## Versions
- Backend-5.11.8.dll
- Backend-1.0.9.aar
## 5.11.8 Update
**[Updated]**  
- [유저 관리] 페데레이션 뒤끝 가입 여부 함수 CheckUserInBackend가 GPGS2, AppleWebLogin, Steam 가입을 확인할 수 있도록 개선되었습니다.
- [ToolKit] 안드로이드 GoogleLogin 메인/외부 쓰레드 콜백 선택 기능이 추가되었습니다.  
- [ToolKit] 안드로이드 AppleLogin 메인/외부 쓰레드 콜백 선택 기능이 추가되었습니다.  

**[Fixed]**
- [게임 정보] PlayerData 함수를 통해 Insert 할 때 스키마 테이블에서 삽입값이 0일 경우, 기본값으로 삽입되는 에러가 픽스되었습니다.


## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| EngineIoClientDotNet 0.9.22   | 0.9.22     | The MIT License                     |
| SocketIoClientDotNet 0.9.13   | 0.9.13     | The MIT License                     |
| ILRepack.Lib.MSBuild.Task     | 2.0.18.1   | The MIT License                     |
| Newtonsoft.Json 12.0.3        | 12.0.3     | The MIT License                     |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
