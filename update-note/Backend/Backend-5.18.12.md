---
title: Backend-5.18.12
date: 2026-07-09T10:00
slug: backend-5-18-12
---

:::danger 5.18.12 지원 종료
5.18.12부터 5.18.14까지 숫자로만 구성된 문자열 키가 포함된 Dictionary를 일부 요청에 전달할 경우 401 `bad signature` 오류가 발생할 수 있습니다.<br />
문제가 수정된 5.18.16으로 업데이트해 주세요.
:::

:::info 업데이트 요약
[데이터 처리] JSON 직렬화 및 파싱 안정화 작업이 진행되었습니다.
:::

<!--truncate-->


## Versions
- Backend-5.18.12.dll
- Backend-1.1.0.aar

## 5.18.12 Update
**[Fixes]**
- [데이터 처리] JSON 직렬화 및 파싱 안정화 작업이 진행되었습니다.
   - JSON 직렬화 및 파싱 과정의 안정성을 개선하였습니다.

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
