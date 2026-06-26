# sdk-docs 개요

TheBackend SDK 문서 모음. 각 서브디렉터리는 독립적인 SDK 모듈입니다.

## 모듈 목록

| 디렉터리 | 모듈명 | 대상 엔진 | 설명 |
|---|---|---|---|
| [backend-new/](./backend-new/) | 베이스 SDK | Unity | 게임 서버 핵심 기능 전반 |
| [chat/](./chat/) | 채팅 SDK | Unity | 실시간 채팅 |
| [function/](./function/) | 뒤끝펑션 | Unity | 서버리스 커스텀 함수 |
| [world/](./world/) | 월드 | Unity | 지속형 멀티플레이어 서버 |
| [database/](./database/) | 데이터베이스 | Unity | LINQ 기반 게임 데이터 저장 |
| [unreal-chat/](./unreal-chat/) | 언리얼 채팅 | Unreal Engine | Unreal용 실시간 채팅 |

## 의존 관계

```
베이스 SDK (backend-new)
    ├── 채팅 SDK (chat) — 함께 사용 가능, dll 중복 주의
    ├── 뒤끝펑션 (function) — 베이스 SDK 필수
    ├── 월드 (world) — 독립적, 별도 설치
    └── 데이터베이스 (database) — 독립적, 별도 설치

언리얼 채팅 (unreal-chat) — Unreal Engine 전용, 독립적
```
