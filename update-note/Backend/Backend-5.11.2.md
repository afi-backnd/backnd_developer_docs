---
title: Backend-5.11.2
date: 2023-10-31T10:00
slug: backend-5-11-2
---

:::danger 치명적인 오류가 포함된 SDK입니다!
해당 SDK는 압축형 데이터를 포함한 테이블을 불러올 경우, inDate가 비정상적으로 불러오는 치명적인 오류가 존재합니다.  
해당 에러가 수정된 <a href="https://developer.thebackend.io/sdk/unityPackage/5.11.4/Backend-5.11.4.unitypackage" id="download-sdk">Backend-5.11.4.unitypackage</a>를 사용해주세요.
:::
<!--truncate-->
[SDK .NET 4 버전] ~~다운로드~~


## Versions
- Backend-5.11.2.dll
- Backend-1.0.9.aar

## 5.11.2 Update

* [유저 데이터 관리] 게임 정보 관리의 사용법을 쉽게 바꾼 '유저 데이터 관리' 함수가 추가되었습니다.   
  * 기존 게임 정보관리(GameData)와 동일한 기능입니다.  
  * 함수의 매개변수와 returnValue가 변경되었습니다.(예시 : Get의 returnValue : row[] → rows[])  
  * 함수 호출이 성공할 경우, 해당 요청에 사용된 읽기량과 쓰기량이 표시됩니다.  

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| EngineIoClientDotNet 0.9.22   | 0.9.22     | The MIT License                     |
| SocketIoClientDotNet 0.9.13   | 0.9.13     | The MIT License                     |
| ILRepack.Lib.MSBuild.Task     | 2.0.18.1   | The MIT License                     |
| modernhttpclient 2.4.2        | 2.4.2      | The MIT License                     |
| Newtonsoft.Json 12.0.3        | 12.0.3     | The MIT License                     |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.16.0     | The Unlicense                       |
| .NET Reactor                  | 6.9.0.0    | End-User License Agreement("EULA") |
