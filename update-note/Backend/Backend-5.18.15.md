---
title: Backend-5.18.15
date: 2026-09-01T10:00
slug: backend-5-18-15
---

:::danger 5.18.15 지원 종료
압축 테이블에서 페이징 키를 사용한 다음 페이지 조회가 정상적으로 동작하지 않는 문제가 발견되어 지원이 종료되었습니다.<br />
문제가 수정된 5.18.16으로 업데이트해 주세요.
:::

:::info 업데이트 요약
[요청 인증] 숫자로만 구성된 문자열 키가 포함된 Dictionary를 전달할 때 일부 요청에서 401 오류가 발생할 수 있던 문제가 수정되었습니다.  
[차트] 조회할 차트 또는 확률표가 없을 때 콘텐츠 조회 함수에서 예외가 발생하던 문제가 수정되었습니다.
:::

<!--truncate-->


## Versions
- Backend-5.18.15.dll
- Backend-1.1.0.aar

## 5.18.15 Update
**[Fixes]**
- [요청 인증] 숫자로만 구성된 문자열 키가 포함된 Dictionary를 전달할 때 일부 요청에서 401 오류가 발생할 수 있던 문제가 수정되었습니다.
   - 5.18.12부터 5.18.14까지 `"2"`, `"1"`과 같이 숫자로만 구성된 문자열 키가 포함된 Dictionary를 일부 요청에 전달할 경우 401 `bad signature` 오류가 발생할 수 있었습니다.
   - 해당 문제가 수정되었습니다. 5.18.12부터 5.18.14까지의 버전을 사용 중인 프로젝트는 5.18.16으로 업데이트해 주세요.
- [차트] 빈 목록으로 콘텐츠를 조회할 때 예외가 발생하던 문제가 수정되었습니다.
   - 동기 및 비동기 `Backend.CDN.Content.Get`, `Backend.CDN.Probability.Get`에 빈 목록을 전달하면 클라이언트 예외가 발생하던 문제를 수정하였습니다.
   - 빈 목록을 전달하면 성공 응답과 빈 콘텐츠 목록을 반환합니다. [[차트 내용 조회]](/sdk-docs/backend/base/chart/chart-content) [[뽑기 확률 내용 조회]](/sdk-docs/backend/base/probability/probability-content)

## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
