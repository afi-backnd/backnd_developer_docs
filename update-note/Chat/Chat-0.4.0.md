---
title: Chat-0.4.0
date: 2023-12-20T10:00
slug: chat-0-4-0
---

:::danger 0.4.0 지원 종료
해당 버전은 더 이상 지원되지 않습니다.
:::

<!--truncate-->

[SDK .NET 4 버전] ~~다운로드~~

## Versions
- BackendChat-0.4.0.dll

## 0.4.0 Update

**[Updated]**
- 에러 메시지 고도화 작업이 진행 되었습니다.
- 채널 생성 함수명이 좀 더 명확한 이름으로 변경 되었습니다.
    - SendCreateChannel → SendCreatePrivateChannel

**[Fixed]**
- 중복된 ID로 프라이빗 채널을 생성할 때 오류를 반환하도록 채널 생성 로직을 수정하였습니다.
- IsConnected, IsReConnecting 변수의 접근 제어가 수정 되었습니다.
