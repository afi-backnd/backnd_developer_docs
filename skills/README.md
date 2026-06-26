# TheBackend AI Skills

AI agent가 TheBackend 문서를 탐색하고 활용할 때 사용할 수 있는 스킬 정의 모음입니다.
각 스킬은 자연어 설명과 JSON Schema 형식으로 정의되어 있습니다.

> 머신이 읽을 수 있는 전체 정의: [skills.json](./skills.json)

---

## 스킬 목록

| 스킬 | 설명 | 파일 |
|---|---|---|
| `search_docs` | 키워드로 관련 문서 경로 찾기 | [search_docs.md](./search_docs.md) |
| `get_sdk_info` | 특정 SDK 모듈 정보 조회 | [get_sdk_info.md](./get_sdk_info.md) |
| `get_code_example` | 기능별 코드 예제 반환 | [get_code_example.md](./get_code_example.md) |
| `get_error_info` | 에러 코드 해석 및 해결 방법 | [get_error_info.md](./get_error_info.md) |
| `get_latest_version` | 모듈별 최신 버전 확인 | [get_latest_version.md](./get_latest_version.md) |
| `get_changelog` | 특정 모듈의 버전별 변경 이력 | [get_changelog.md](./get_changelog.md) |

---

## 사용 방법

이 레포를 탐색하는 AI agent는 아래 순서로 접근하는 것을 권장합니다:

1. [`/llms.txt`](../llms.txt) — 전체 문서 구조 파악
2. [`/llms-full.txt`](../llms-full.txt) — 핵심 내용 빠른 참조
3. 관련 모듈의 `_overview.md` — 해당 디렉터리 상세 구조 파악
4. 개별 `.md` 파일 — 구체적인 API/코드 확인

---

## MCP 서버 (예정)

현재 스킬은 문서 정의 형태입니다. 
향후 MCP(Model Context Protocol) 서버를 통해 실제로 callable한 도구로 제공될 예정입니다.
