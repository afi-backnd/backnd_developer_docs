# get_error_info

에러 코드 또는 에러 메시지를 입력하면 원인과 해결 방법을 반환합니다.

## 설명

TheBackend SDK에서 발생하는 에러에 대한 설명과 해결 방법을 반환합니다.

## 입력 파라미터

| 파라미터 | 타입 | 필수 | 설명 |
|---|---|---|---|
| `error` | string | ✓ | 에러 코드 또는 에러 메시지 |
| `module` | string | - | SDK 모듈 한정 |

## 출력

```json
{
  "error_code": "400",
  "description": "잘못된 요청",
  "cause": "파라미터 형식이 올바르지 않음",
  "solution": "Param 객체의 키/값 형식을 확인하세요",
  "reference_path": "sdk-docs/backend-new/02-base/08-common-errors/"
}
```

## 주요 에러 참조 경로

| 에러 유형 | 경로 |
|---|---|
| 전체 에러 코드 목록 | sdk-docs/backend-new/02-base/09-all-errors.md |
| 자주 발생하는 에러 | sdk-docs/backend-new/02-base/08-common-errors/ |
| 채팅 SDK 에러 | sdk-docs/chat/12-error.md |
| 언리얼 채팅 에러 | sdk-docs/unreal-chat/12-error.md |
| 일반 Q&A | qna/troubleshooting.md |

## 자주 발생하는 에러 요약

| 상황 | 원인 | 해결 |
|---|---|---|
| 초기화 실패 | 앱 ID / 클라이언트 키 불일치 | 콘솔에서 키 재확인, 공백 제거 |
| 채팅 dll 충돌 | 베이스 SDK + 채팅 SDK 동시 임포트 | 채팅 SDK 임포트 시 Backend.dll 체크 해제 |
| 안드로이드 빌드 실패 | AGP 버전 8.5 이하 | mainTemplate.gradle 추가 설정 |
| 펑션 인증 실패 | Function Auth Key 미입력 | Edit Settings에서 키 입력 |
| 랭킹 업데이트 안됨 | 테이블 비활성화 | 콘솔에서 랭킹 테이블 활성화 확인 |
