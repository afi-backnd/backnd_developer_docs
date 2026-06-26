---
title: Backend-5.11.0
date: 2023-07-25T10:00
slug: backend-5-11-0
---

:::danger 치명적인 오류가 포함된 SDK입니다!
해당 SDK는 압축형 데이터를 포함한 테이블을 불러올 경우, inDate가 비정상적으로 불러오는 치명적인 오류가 존재합니다.  
해당 에러가 수정된 <a href="https://developer.thebackend.io/sdk/unityPackage/5.11.4/Backend-5.11.4.unitypackage" id="download-sdk">Backend-5.11.4.unitypackage</a>를 사용해주세요.
:::
<!--truncate-->
[SDK .NET 4 버전] ~~다운로드~~


## Versions
- Backend-5.11.0.dll
- Backend-1.0.9.aar

## 5.11.0 Update

- [국가 기능] 국가 기능이 추가되었습니다.  
- [게임 정보 관리] DB 자동 압축화 기능이 내부적으로 추가되었습니다.  
- [Inspector] 프로젝트를 여러 개 등록할 수 있는 기능이 추가되었습니다.  
- [랭킹 관리] extraData(추가항목)이 최대 256byte까지 입력가능하도록 수정되었습니다.  
- [랭킹 관리] 최대로 불러올 수 있는 limit이 50, gap이 25로 수정되었습니다.  
- [뒤끝챗] 필터링에 차단한 닉네임이 포함된 다른 닉네임도 차단되는 에러가 픽스되었습니다.  


## SDK 포함 Nuget

| nuget 이름                    | 버전       | 라이센스                    |
| ----------------------------- | ---------- | --------------------------- |
| EngineIoClientDotNet 0.9.22   | 0.9.22     | The MIT License             |
| SocketIoClientDotNet 0.9.13   | 0.9.13     | The MIT License             |
| ILRepack.Lib.MSBuild.Task     | 2.0.18.1   | The MIT License             |
| modernhttpclient 2.4.2        | 2.4.2      | The MIT License             |
| Newtonsoft.Json 12.0.3        | 12.0.3     | The MIT License             |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0 |
| LitJson                       | 0.16.0     | The Unlicense               |
| RedGate.SmartAssembly.MSBuild | 8.1.2.4975 | Redgate Master EULAs        |
