---
sidebar_label: 베이스 SDK와 함께 사용하는 경우
description: "베이스 SDK와 함께 사용하는 경우"
---

# 베이스 SDK와 함께 사용하는 경우
이 문서는 뒤끝 채팅 SDK를 베이스 SDK와 함께 사용하는 경우 반드시 확인해야 할 내용을 안내합니다.

:::note 참고: 채팅 SDK만 단독으로 사용하실 수 있습니다.
뒤끝 채팅 SDK에는 유저 인증 기능이 포함된 `Backend.dll`이 기본 내장되어 있습니다.  
따라서 베이스 SDK를 별도로 설치하지 않아도, 채팅만 단독으로 사용할 수 있습니다.
:::

### 채팅 SDK를 먼저 임포트하는 경우

1. 채팅 SDK를 Import 합니다.
2. 베이스 SDK **5.11.9 이상** 버전을 Import 해서, 채팅에 포함되어 있는 `Backend.dll`을 덮어씌웁니다.

### 베이스 SDK를 먼저 임포트하는 경우
1. 베이스 SDK **5.11.9 이상** 버전을 Import 합니다.
2. 채팅 SDK를 임포트할 때, `Backend.dll`이 덮어씌워지지 않도록 **체크 해제**합니다.
![import chat sdk](/img/docs/guide/chat/install/import-chat-sdk.png)

### 관련 링크

- [채팅 시작하기](/sdk-docs/chat/intro)
- [베이스 시작하기](/sdk-docs/backend/02-base/start-up)
