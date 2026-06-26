# api-docs 개요

TheBackend 플랫폼 REST API 레퍼런스. 서버에서 직접 호출하는 관리용 API입니다.

## 모듈

| 디렉터리 | 설명 |
|---|---|
| [chat/](./chat/) | 채팅 서버 관리 API |

## chat/ API

**Base URL**: `https://platformapi.thebackend.io/chat/v1`

| 파일 | 내용 |
|---|---|
| [01-intro.md](./chat/01-intro.md) | API 소개 및 인증 방법 |
| [02-channel.md](./chat/02-channel.md) | 채널 관리 (생성/삭제/목록) — 현재 오픈 채널만 지원 |
| [03-message.md](./chat/03-message.md) | 시스템 메시지 전송 |
| [04-report.md](./chat/04-report.md) | 신고 목록 조회 |
| [05-blocking.md](./chat/05-blocking.md) | 유저 제재/채팅 차단 |
| [06-filter.md](./chat/06-filter.md) | 욕설 필터 관리 |
