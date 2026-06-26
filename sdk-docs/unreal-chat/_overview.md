# 언리얼 채팅 SDK 개요

Unreal Engine 전용 실시간 채팅 SDK. Unity 채팅 SDK와 동일한 기능을 C++로 제공합니다.
커스텀 인증만 지원합니다 (소셜 로그인 미지원).

## 파일 목록

| 파일 | 내용 |
|---|---|
| [01-intro.md](./01-intro.md) | 시작하기 |
| [02-install.md](./02-install.md) | 설치 |
| [03-example-game.md](./03-example-game.md) | 예제 게임 |
| [04-custom-auth.md](./04-custom-auth.md) | 커스텀 인증 (필수) |
| [05-user.md](./05-user.md) | 유저 관리 |
| [06-channel.md](./06-channel.md) | 채널 관리 |
| [07-message.md](./07-message.md) | 메시지 |
| [08-report.md](./08-report.md) | 신고 |
| [09-translate.md](./09-translate.md) | 번역 |
| [10-quit.md](./10-quit.md) | 종료 |
| [11-success.md](./11-success.md) | 성공 응답 |
| [12-error.md](./12-error.md) | 에러 코드 |

## Unity 채팅 SDK와 차이점
- 대상 엔진: Unreal Engine (C++)
- 인증: 커스텀 인증만 지원
- 함수명 prefix: `Backnd` (Backend → Backnd로 변경, v1.2.0 이후)

## 최신 버전
Unreal Chat 1.3.0 — [릴리즈 노트](../../update-note/UnrealChat/)
