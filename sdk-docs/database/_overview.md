# 데이터베이스 SDK 개요

LINQ 기반 타입 안전 게임 데이터베이스. 역할 기반 접근 제어와 트랜잭션을 지원합니다.

## 파일 목록

| 파일 | 내용 |
|---|---|
| [01-intro.md](./01-intro.md) | 소개 — UserTable/FlexibleTable, 접근 제어 개념 |
| [02-data-modeling.md](./02-data-modeling.md) | 스키마 설계 및 데이터 모델링 |
| [03-insert.md](./03-insert.md) | 데이터 삽입 |
| [04-query.md](./04-query.md) | 데이터 조회 (LINQ 쿼리) |
| [05-update-delete.md](./05-update-delete.md) | 데이터 수정/삭제 |
| [06-transaction.md](./06-transaction.md) | 트랜잭션 (읽기/쓰기) |
| [07-supported-operators.md](./07-supported-operators.md) | 지원 연산자 목록 |
| [08-samples.md](./08-samples.md) | 코드 예제 |

## 테이블 타입

| 타입 | 설명 |
|---|---|
| UserTable | 유저별 독립 데이터 (개인 인벤토리 등) |
| FlexibleTable | 여러 유저가 공유하는 데이터 (길드 정보 등) |

## 접근 제어 역할
- `ClientAccess`: 클라이언트에서 직접 접근 허용
- `Read`: 읽기만 허용
- `Write`: 쓰기 허용

## 최신 버전
Database 1.0.0 — [릴리즈 노트](../../update-note/Database/)
