---
sidebar_label: 시작하기
---

# 시작하기
채팅 서버의 데이터를 직접 관리할 수 있도록 라이브옵스 기능을 API 형태로 제공합니다. 별도의 운영 대시보드를 구축한 경우, Platform API를 직접 연결하여 보다 효과적으로 게임을 운영할 수 있습니다.  
제공되는 API는 표준 HTTP 프로토콜을 사용하며, HTTP 요청에 대한 응답으로 JSON 페이로드를 반환합니다.

:::info
Platform API는 클라이언트 사이드 기능이 아닙니다. 클라이언트 기능을 구현하기 위해서는 SDK를 사용하세요.
:::

## 채팅 API 사용 방법

### Base URL

아래 URL로 API 요청을 보내세요.
```js
https://platformapi.thebackend.io/chat/v1
```

### Headers
모든 API에 대한 일반적인 HTTP 요청에는 다음 헤더가 포함됩니다.  
Master API Token을 통해 요청을 인증하고 데이터와 상호작용할 권한이 있음을 보장하세요. 토큰 값은 뒤끝 콘솔에서 확인할 수 있습니다.
```js
Content-Type: application/json; charset=utf8
Authorization: Bearer YOUR_MASTER_TOKEN
```

### 요청
다음은 API 요청 예시입니다.
이 요청은 오픈 채널 그룹 목록이 담긴 JSON 페이로드를 반환합니다.
```shell
curl -X GET https://platformapi.thebackend.io/chat/v1/open_channel_groups \
-H "Authorization: Bearer YOUR_MASTER_TOKEN" \
-H "Content-Type: application/json; charset=utf8" \
```

사용할 수 있는 모든 기능과 엔드 포인트에 대한 자세한 내용은 이어지는 API 문서에서 확인하세요.
