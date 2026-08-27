---
title: Backend-5.16.0
date: 2024-12-10T10:00
slug: backend-5-16-0
---

:::danger 5.16.0 지원 종료
특정 상황에서 이용에 불편을 주는 에러가 발견되어 해당 버전의 지원이 종료되었습니다.  
- [게임데이터] Where의 Between 조건 사용 시, iOS에서 에러 발생 이슈.  
- [CDN] CDN 차트에서 ContentItem의 Json데이터 변환 오류 발생 이슈.  

해당 이슈는 5.16.1 버전에서 수정이 되었으므로 5.16.1 버전을 사용해 주시기 바랍니다.
:::

:::info 업데이트 요약
클라우드 세이브 기능이 추가되었습니다.  
CDN 차트에서 로컬 삭제 기능이 추가되었습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.16.0.dll
- Backend-1.0.9.aar

## 5.16.0 Update

**[Updates]**
- 클라우드 세이브 기능이 추가되었습니다. [[개발자문서]](/sdk-docs/backend/base/cloud-save/get-collections)  
  - 원격 저장소에 JSON 문자열 형태로 데이터를 업로드하고 다운로드 받을 수 있습니다.
- CDN 차트를 통해 로컬에 저장한 차트를 삭제하는 기능이 추가되었습니다. [[개발자문서]](/sdk-docs/backend/base/chart/Local/delete)  


## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
