---
title: Backend-5.11.9
date: 2024-06-25T10:00
slug: backend-5-11-9
---

:::info 업데이트 요약
애플 토큰 만료 기능이 추가되었습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] <a href="https://developer.thebackend.io/sdk/unityPackage/5.11.9/Backend-5.11.9.unitypackage" target="_blank">다운로드</a>   

## Versions
- Backend-5.11.9.dll
- Backend-1.0.9.aar

## 5.11.9 Update
**[Updated]**  
- [영수증 검증] 원스토어 영수증 검증 시, 해외 결제와 국내 결제를 선택할 수 있는 기능이 추가되었습니다.  
- [유저 관리] 애플 토큰 만료 기능이 추가되었습니다.  
iOS 환경에서는 뒤끝 회원 탈퇴 전에 반드시 해당 함수를 호출해야합니다.  

**[Fixed]**
- [채팅] 뒤끝 채팅과의 연결 로직이 개선되었습니다.  
BackendChat-1.2.0 이상부터는 5.11.9 이상 버전과 함께 사용해야 합니다.  


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
