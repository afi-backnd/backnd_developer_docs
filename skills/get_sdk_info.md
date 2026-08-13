# get_sdk_info

특정 SDK 모듈의 개요, 주요 기능, 최신 버전, 참조 경로를 반환합니다.

## 설명

SDK 모듈 이름을 입력하면 해당 모듈의 핵심 정보를 구조화된 형태로 반환합니다.

## 입력 파라미터

| 파라미터 | 타입 | 필수 | 설명 |
|---|---|---|---|
| `module` | string | ✓ | SDK 모듈명 |

**module 허용값**:
- `backend` — 베이스 SDK
- `chat` — Unity 채팅 SDK
- `function` — 뒤끝펑션
- `world` — 월드
- `database` — 데이터베이스
- `unreal-chat` — 언리얼 채팅 SDK
- `api-chat` — 채팅 REST API

## 출력

```json
{
  "module": "chat",
  "name": "채팅 SDK",
  "description": "Unity 실시간 채팅 SDK",
  "latest_version": "1.4.0",
  "engine": "Unity",
  "overview_path": "sdk-docs/chat/_overview.md",
  "docs_path": "sdk-docs/chat/",
  "changelog_path": "update-note/Chat/",
  "features": ["채널", "메시지", "번역", "신고"],
  "dependencies": ["베이스 SDK (선택적)"],
  "notes": ["베이스 SDK 병용 시 Backend.dll 중복 주의"]
}
```

## 모듈별 참조 경로

| module | overview | docs | changelog |
|---|---|---|---|
| backend | sdk-docs/backend/_overview.md | sdk-docs/backend/ | update-note/Backend/ |
| chat | sdk-docs/chat/_overview.md | sdk-docs/chat/ | update-note/Chat/ |
| function | sdk-docs/function/_overview.md | sdk-docs/function/ | update-note/Function/ |
| world | sdk-docs/world/_overview.md | sdk-docs/world/ | update-note/World/ |
| database | sdk-docs/database/_overview.md | sdk-docs/database/ | update-note/Database/ |
| unreal-chat | sdk-docs/unreal-chat/_overview.md | sdk-docs/unreal-chat/ | update-note/UnrealChat/ |
| api-chat | api-docs/_overview.md | api-docs/chat/ | update-note/ApiChat/ |
