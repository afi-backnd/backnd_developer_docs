# 활용 : 길드 유저간 리더보드 구성

**데이터베이스의 유저 테이블을 활용하여 길드 내 유저 간 주간 리더보드를 구현할 수 있습니다.**  
본 문서는 테이블 설계부터 보상 처리까지 전체 흐름을 정리한 예시입니다.

---

## 기본 조건 예시

- 주간 단위 리더보드 운영
- 기여도 기반 순위 산정
- 보상 정책
  - 1 ~ 3위: 순위 보상 지급
  - 전체 유저: 참여 보상 지급
- 테이블은 자신/타인의 데이터를 모두 읽고 쓸수 있도록 설정

## 설계 포인트

- `week_key` 기준 데이터 분리 (주간 리셋 구조)  
- `null → false → true` 흐름으로 보상 상태 관리  
- 조회 시점 초기화를 통해 별도 배치 작업 최소화  
- INDEX 키의 조합으로 빠른 조회 가능  

---

## 테이블 설계

### 컬럼 구성 상세

| 컬럼명   | 타입 | 기본값 | INDEX 설정 | Nullable 설정 | 설명 |
|--------|------|------|--------|----------|------|
| user_uuid | uuid | 자동 | 자동 | - | 유저 고유 식별자 (기본 제공 컬럼) |
| id | int32 | 자동 | - | - | PK |
| owner_indate | string | - | 설정 | 설정 | 유저 식별자 |
| guild_indate | string | - | 설정 | 설정 | 길드 식별자 |
| week_key | string | - | 설정 | 설정 | 주차 키 (예: 2026-W15) |
| score | int64 | 0 | - | - | 누적 기여도 |
| rank_reward | bool | - | - | 설정 | 순위 보상 수령 여부 |
| rank | int32 | - | - | 설정 | 보상 판단에 사용하는 확정 순위 | 
| all_reward | bool | - | - | 설정 | 전체 보상 수령 여부 |
| updated_at | datetime | NOW()| -  | - | 마지막 업데이트 시각 (선택 사용) |

### 상태 값 정의

| 필드 | 값 | 의미 |
|------|----|------|
| rank_reward, all_reward | null | 보상 지급 여부 미결정 |
| rank_reward, all_reward | false | 보상 수령 가능 |
| rank_reward, all_reward | true | 보상 수령 완료 / 보상 수령 미대상 |

---

## 주요 처리 흐름

### 1. 스코어 갱신

기여 발생 시:

- 현재 `week_key` 기준 데이터 조회
- 데이터가 없으면 INSERT
  >`rank_reward`, `rank`, `all_reward`는 기본값이 `null`이므로 별도 지정하지 않아도 됩니다.  
  단, `uuid`, `guild_indate`, `week_key`는 기본값은 `null`이지만 식별 및 조회에 사용되는 필드이므로 반드시 값을 입력해야 합니다.
- 데이터가 있으면 `score` 누적

```sql
-- 없는 경우
INSERT (owner_indate, guild_indate, week_key, score)

-- 있는 경우
UPDATE score += 기여값
```

---

### 2. 리더보드 조회

#### 현재 주 조회

- 현재 `week_key` 계산
- `guild_indate` + `week_key` 조건으로 조회
- `score DESC` 정렬
- 순위 표시

#### 지난 주 조회

- 이전 `week_key` 계산
- `guild_indate` + 이전 `week_key` 조건으로 조회
- `score DESC` 정렬
- 순위 표시

#### 지난 주 조회 시 보상 상태 초기화

- 본인 row의 `rank_reward == null` 인 경우
  - 이전 `week_key`의 데이터를 `score DESC` 기준으로 정렬
  - 자신의 순위를 계산하여 `rank` 컬럼에 저장
  - 계산된 본인 순위가 1 ~ 3위인 경우 `rank_reward = false`
  - 계산된 본인 순위가 4위 이하인 경우 `rank_reward = true`
    > `rank` 컬럼은 지난 주 보상 판단을 위한 확정 순위 저장 용도로 사용하며, 현재 주 순위 표시는 조회 결과 기준으로 계산하여 노출합니다.

- 본인 row의 `all_reward == null` 인 경우
  - `all_reward = false`로 변경

#### 과거 이력 조회 (선택)

- 이전 `week_key` 기준 조회
- `score DESC` 정렬
- 보상 버튼 미노출

---

### 3. 보상 처리

#### 순위 보상 (rank_reward)

| 상태 | 동작 |
|------|------|
| null | 최초 접근 시 자신의 순위를 계산하여 rank 컬럼 저장 및 false로 변경 |
| false | 클릭 시 rank 컬럼 정보에 따라 보상 지급 후 true로 변경 |
| true | 버튼 비활성 |

#### 전체 보상 (all_reward)

| 상태 | 동작 |
|------|------|
| null | 최초 접근 시 false로 변경 |
| false | 클릭 시 보상 지급 후 true로 변경 |
| true | 버튼 비활성 |

---

### 4. 길드 탈퇴/추방 처리

#### 길드 탈퇴 시
- `guild_indate` + `week_key` + `owner_indate` 조건으로 데이터 조회
- 해당 데이터 삭제

#### 길드 추방 시
- 추방 대상 유저의 owner_indate 확인
- `guild_indate` + `week_key` + 추방 유저 `owner_indate` 조건으로 데이터 조회
- 해당 데이터 삭제

---

### 5. 데이터 정리 (선택)

- 일정 기간 이상 지난 `week_key` 데이터  
- 콘솔 쿼리 에디터를 통해 일괄 삭제  
