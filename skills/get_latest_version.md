# get_latest_version

모듈명을 입력하면 최신 버전 정보와 다운로드 경로를 반환합니다.

## 설명

TheBackend SDK 모듈의 최신 버전 정보를 반환합니다.
버전 정보는 배포 파이프라인에 의해 자동으로 동기화됩니다.

## 입력 파라미터

| 파라미터 | 타입 | 필수 | 설명 |
|---|---|---|---|
| `module` | string | ✓ | SDK 모듈명 (backend, chat, function, world, database, unreal-chat) |

## 현재 버전 정보

| 모듈 | 최신 버전 | 릴리즈 노트 경로 |
|---|---|---|
| Backend SDK | 5.11.x | update-note/Backend/ |
| Chat SDK | 1.4.0 | update-note/Chat/ |
| Function | 1.0.x | update-note/Function/ |
| World | 0.2.0 | update-note/World/ |
| Database | 1.0.0 | update-note/Database/ |
| Unreal Chat | 1.3.0 | update-note/UnrealChat/ |
| Chat API | 1.0.0 | update-note/ApiChat/ |

> 정확한 최신 버전은 각 모듈의 릴리즈 노트 디렉터리에서 가장 높은 버전 파일명을 확인하세요.

## 출력

```json
{
  "module": "chat",
  "latest_version": "1.4.0",
  "release_date": "2025-12-18",
  "changelog_path": "update-note/Chat/Chat-1.4.0.md",
  "download_url": "https://developer.thebackend.io/sdk/chat/1.4.0/BackndChat-1.4.0.unitypackage"
}
```
