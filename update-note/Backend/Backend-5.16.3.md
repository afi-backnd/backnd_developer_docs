---
title: Backend-5.16.3
date: 2025-04-16T10:00
slug: backend-5-16-3
---

:::danger 5.16.3 지원 종료
특정 상황에서 사용에 문제가 있는 에러가 발견되어 해당 버전의 지원이 종료되었습니다.  
- [클라우드 세이브] 특정 데이터 형식이 클라우드 세이브로 업로드 되지 않던 문제를 수정했습니다.   

해당 이슈는 5.17.1 버전에서 수정이 되었으므로 5.17.1 버전을 사용해 주시기 바랍니다.
:::

:::info 업데이트 요약
[데이터 처리] 언마셜링이 문화권에 영향을 받지 않도록 수정했습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.16.3.dll
- Backend-1.0.9.aar

## 5.16.3 Update
**[Fixes]**
- GetFlattenJSON()같은 언마셜링을 하는 함수에서 숫자 데이터 변환시, 문화권에 영향을 받지 않도록 수정했습니다.  

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
