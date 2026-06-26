# search_docs

키워드로 관련 문서 경로와 설명을 반환합니다.

## 설명

주어진 키워드와 관련된 TheBackend 문서 파일 목록을 반환합니다.
각 결과에는 파일 경로, 문서 제목, 관련도 설명이 포함됩니다.

## 입력 파라미터

| 파라미터 | 타입 | 필수 | 설명 |
|---|---|---|---|
| `query` | string | ✓ | 검색 키워드 (한국어/영어 모두 가능) |
| `module` | string | - | 특정 모듈로 검색 범위 제한 (backend, chat, function, world, database, unreal-chat, api) |
| `limit` | integer | - | 반환할 최대 결과 수 (기본값: 5) |

## 출력

```json
{
  "results": [
    {
      "path": "sdk-docs/backend-new/02-base/27-guild/",
      "title": "길드 시스템",
      "description": "길드 생성, 검색, 멤버 관리, 길드 랭킹",
      "module": "backend"
    }
  ]
}
```

## 예시

**입력**: `{ "query": "길드 생성", "module": "backend" }`

**참조 경로**:
- `sdk-docs/backend-new/02-base/27-guild/` — 길드 API 전체
- `guide/console-guide/04-backnd-base/12-guild/` — 콘솔에서 길드 설정
- `qna/backend-sdk.md` — 길드 관련 Q&A

**입력**: `{ "query": "push notification android" }`

**참조 경로**:
- `sdk-docs/backend-new/02-base/16-push/` — 푸시 알림 SDK
- `guide/console-guide/02-server-setting/04-push/` — 콘솔 푸시 설정
- `faq/08-push.md` — 푸시 FAQ
