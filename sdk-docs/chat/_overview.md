# 채팅 SDK 개요

Unity 실시간 채팅 SDK. 채널 기반 메시지 시스템을 Unity 게임에 빠르게 연동합니다.

## 파일 목록

| 파일 | 내용 |
|---|---|
| [01-intro.md](./01-intro.md) | 시작하기 — 설치, 첫 메시지까지 |
| [02-install/](./02-install/) | 설치 상세 (베이스 SDK 병용 시 주의사항 포함) |
| [03-example-game.md](./03-example-game.md) | 예제 프로젝트 |
| [04-custom-auth.md](./04-custom-auth.md) | 커스텀 인증 설정 |
| [05-user.md](./05-user.md) | 유저 관리 |
| [06-channel.md](./06-channel.md) | 채널 생성/참여/나가기 |
| [07-message.md](./07-message.md) | 메시지 전송/수신 |
| [08-report.md](./08-report.md) | 유저 신고 |
| [09-translate.md](./09-translate.md) | 메시지 자동 번역 |
| [10-quit.md](./10-quit.md) | 채팅 종료 |
| [11-success.md](./11-success.md) | 성공 응답 코드 |
| [12-error.md](./12-error.md) | 에러 코드 |

## 주의사항
- 베이스 SDK와 함께 사용 시 임포트 때 `Backend.dll` 체크 해제 필요
- 안드로이드 AGP 8.5 이하: `mainTemplate.gradle` 추가 설정 필요

## 최신 버전
Chat SDK 1.4.0 — [릴리즈 노트](../../update-note/Chat/)
