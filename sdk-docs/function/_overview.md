# 뒤끝펑션 개요

서버리스 커스텀 함수 실행 환경. 보안이 필요한 서버 로직을 클라우드에서 직접 실행합니다.
베이스 SDK에서 함수를 호출하는 구조이며, 단독 사용은 불가능합니다.

## 파일 목록

| 파일/폴더 | 내용 |
|---|---|
| [01-intro.md](./01-intro.md) | 시작하기 — 인증키 발급, 설치 |
| [02-migrate-from-old-version.md](./02-migrate-from-old-version.md) | 구버전 마이그레이션 |
| [03-basic/](./03-basic/) | 기본 함수 호출, 타임아웃, 응답 시간, 빌드 |
| [04-work-in-windows/](./04-work-in-windows/) | Windows 개발 환경 (Visual Studio, VS Code) |
| [05-work-in-ios/](./05-work-in-ios/) | macOS 개발 환경 (VS Code) |
| [06-cli/](./06-cli/) | backend CLI — 배포, 빌드, 삭제, 설정 |
| [07-upgrade.md](./07-upgrade.md) | 버전 업그레이드 |
| [08-template.md](./08-template.md) | 함수 템플릿 |
| [09-tutorial/](./09-tutorial/) | 에코 함수, 영수증 검증 튜토리얼 |
| [10-connect-with-base/](./10-connect-with-base/) | 베이스 SDK 연동 (데이터 로드/저장) |
| [11-transfer-stream/](./11-transfer-stream/) | 스트림 전송 (String, JSON) |
| [12-request/](./12-request/) | 요청 처리, Apple/Google 영수증 검증 |

## 주요 사용 사례
- 영수증 검증 (클라이언트 위변조 방지)
- 확률 연산 서버 처리 (가챠 해킹 방지)
- 여러 SDK 기능을 원자적으로 조합 실행
- 외부 API 호출

## 최신 버전
Function 1.0.x — [릴리즈 노트](../../update-note/Function/)
