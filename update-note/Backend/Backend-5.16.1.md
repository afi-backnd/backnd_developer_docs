---
title: Backend-5.16.1
date: 2025-01-10T10:00
slug: backend-5-16-1
---

:::danger 5.16.1 지원 종료
특정 상황에서 이용에 불편을 주는 에러가 발견되어 해당 버전의 지원이 종료되었습니다.  
- [클라우드 세이브] 업로드 시, 제한 용량 계산에 오류가 있던 문제.  

해당 이슈는 5.16.2 버전에서 수정이 되었으므로 5.16.2 버전을 사용해 주시기 바랍니다.
:::

:::info 업데이트 요약
[게임데이터] Where의 Between 조건 사용 시, iOS에서 에러가 발생하던 문제를 수정했습니다.  
[CDN] CDN 차트에서 ContentItem의 Json데이터 변환 오류를 수정했습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.16.1.dll
- Backend-1.0.9.aar

## 5.16.1 Update
**[Fixes]**
- 게임 정보 조회에서 Where 조건 중, Between을 사용하면  
iOS에서 Malformed URL 오류가 발생하던 문제를 수정했습니다.  
- CDN 차트에서 ContentItem의 contentJson으로 가져온 JsonData를  
foreach에서 사용할 때, InvalidOperationException이 발생하던 문제를 수정했습니다.  


## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
