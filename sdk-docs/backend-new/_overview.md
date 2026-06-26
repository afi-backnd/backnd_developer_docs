# 베이스 SDK 개요

Unity 게임 서버 기능 전반을 담당하는 TheBackend 핵심 SDK.
인증부터 데이터 저장, 소셜 기능, 결제까지 게임 서버에서 필요한 모든 기능을 제공합니다.

## 디렉터리 구조

| 디렉터리 | 설명 |
|---|---|
| [02-base/](./02-base/) | 핵심 기능 전체 (아래 상세 목록 참조) |
| [03-match/](./03-match/) | 멀티플레이어 매치메이킹 서버 |
| [05-toolkit/](./05-toolkit/) | 문의 플러그인, Google/Apple 로그인, 욕설 필터 |

## 02-base/ 기능 목록

| 파일/폴더 | 기능 |
|---|---|
| 01-start-up.md | SDK 연동 시작 가이드 |
| 02-guideline/ | 단계별 튜토리얼 (유저, 게임 정보, 랭킹, 차트, 우편, 쿠폰 등) |
| 05-sdk-initialize.md | SDK 초기화 코드 |
| 06-knowhow/ | 비동기 처리, Param, 외부 플러그인 충돌 등 고급 개념 |
| 08-common-errors/ | 자주 발생하는 에러 목록 |
| 09-all-errors.md | 전체 에러 코드 레퍼런스 |
| 12-player-data/ | 플레이어 데이터 CRUD (Insert/Get/Update/Delete) |
| 13-user/ | 회원가입, 로그인, 소셜 연동, 멀티 캐릭터 |
| 14-leaderboard/ | 유저/길드 랭킹 |
| 15-coupon/ | 쿠폰 발급/사용 |
| 16-push/ | Android/iOS 푸시 알림 |
| 17-game-log/ | 게임 이벤트 로그 |
| 18-chart/ | 서버 기반 아이템 차트 |
| 19-probability/ | 가챠/확률 추첨 |
| 20-receipt/ | 인앱결제 영수증 검증 (Google, Apple, OneStore) |
| 21-cash/ | 유료 재화 관리 |
| 24-friend/ | 친구 시스템 |
| 25-post/ | 우편함 (관리자/유저/랭킹/쿠폰 우편) |
| 26-message/ | 1:1 메시지 |
| 27-guild/ | 길드 시스템 |
| 28-notify/ | 실시간 서버→클라이언트 알림 |
| 31-function/ | 뒤끝펑션 호출 |

## 최신 버전
Backend SDK 5.11.x — [릴리즈 노트](../../update-note/Backend/)
