---
title: Backend-5.11.3
date: 2023-11-28T10:00
slug: backend-5-11-3
---

:::danger 치명적인 오류가 포함된 SDK입니다!
해당 SDK는 압축형 데이터를 포함한 테이블을 불러올 경우, inDate가 비정상적으로 불러오는 치명적인 오류가 존재합니다.  
해당 에러가 수정된 <a href="https://developer.thebackend.io/sdk/unityPackage/5.11.4/Backend-5.11.4.unitypackage" id="download-sdk">Backend-5.11.4.unitypackage</a>를 사용해주세요.
:::
<!--truncate-->
[SDK .NET 4 버전] ~~다운로드~~


## Versions
- Backend-5.11.3.dll
- Backend-1.0.9.aar

## 5.11.3 Update

* [ToolKit] 구글 로그인 SDK가 추가되었습니다.   
  * 안드로이드에서 구글 로그인하는 함수가 추가되었습니다.  
  * 애플에서 구글 로그인하는 함수가 추가되었습니다.  
* [게임유저관리] 스팀 로그인 기능이 추가되었습니다.  
* [우편관리] 만료된 우편의 inDate를 이용한 우편 개별 수령이 가능했던 문제를 픽스했습니다.  
* [구버전 뒤끝챗] 유저 차단 리스트가 초기화되던 문제를 픽스했습니다.  

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
