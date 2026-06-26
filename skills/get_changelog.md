# get_changelog

모듈과 버전을 입력하면 해당 버전의 변경 이력을 반환합니다.

## 설명

특정 모듈의 버전별 변경 사항(신기능, 버그 수정, 브레이킹 체인지)을 반환합니다.

## 입력 파라미터

| 파라미터 | 타입 | 필수 | 설명 |
|---|---|---|---|
| `module` | string | ✓ | SDK 모듈명 |
| `version` | string | - | 특정 버전 (미입력 시 최신 버전) |
| `from_version` | string | - | 범위 조회 시작 버전 |
| `to_version` | string | - | 범위 조회 종료 버전 |

## 파일명 규칙

릴리즈 노트는 `update-note/{Module}/{Module}-{version}.md` 형식입니다.

| 모듈 | 경로 패턴 |
|---|---|
| Backend | update-note/Backend/Backend-{version}.md |
| Chat | update-note/Chat/Chat-{version}.md |
| Function | update-note/Function/Function-{version}.md |
| World | update-note/World/World-{version}.md |
| Database | update-note/Database/Database-{version}.md |
| Unreal Chat | update-note/UnrealChat/UnrealChat-{version}.md |
| Chat API | update-note/ApiChat/ApiChat-{version}.md |

## 예시

**입력**: `{ "module": "chat", "version": "1.4.0" }`

**참조**: `update-note/Chat/Chat-1.4.0.md`

**입력**: `{ "module": "backend", "from_version": "5.10.0", "to_version": "5.11.0" }`

**참조**: `update-note/Backend/` 디렉터리에서 해당 범위 파일들 참조
