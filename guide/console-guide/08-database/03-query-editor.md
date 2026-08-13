---
description: "쿼리 에디터"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 쿼리 에디터

<!-- :::caution 출시 예정
데이터베이스는 **2026년 1월 출시 예정**입니다.
::: -->

쿼리 에디터는 데이터베이스에 저장된 데이터를 SQL-Like 문법으로 직접 조회·수정할 수 있는 도구입니다.  
테이블 에디터에서 제공되는 기본 CRUD 기능보다 더 유연하게 데이터를 다룰 수 있으며,  
조건 검색, 정렬, 범위 지정 등 복잡한 데이터를 빠르게 확인할 때 유용합니다.

![데이터베이스 활성화 페이지](/img/docs/guide/database/database_006.png)

## 화면 구성

- **좌측 패널:** 데이터베이스 내 테이블 목록
- **상단 탭 영역:** 여러 쿼리를 동시에 탭으로 열어 전환할 수 있습니다.
- **우측 상단:** SQL-Like 문법을 입력하는 쿼리 작성 영역입니다.
- **우측 하단:** 쿼리 실행 결과를 테이블 형태로 표시

## 주요 기능

### SELECT

조건 검색, 정렬, 특정 컬럼 선택 등 기본 조회 기능을 지원합니다.

```sql
SELECT * FROM table WHERE level > 10 LIMIT 100;
```

### UPDATE / DELETE

데이터를 직접 수정하거나 삭제할 수 있습니다.

```sql
UPDATE users SET level = 5 WHERE user_uuid = '...';
DELETE FROM logs WHERE created_at < '2025-01-01';
```

### 멀티 탭 실행

여러 개의 쿼리를 탭으로 열어 개별적으로 실행할 수 있습니다.

## 유의사항

- UPDATE/DELETE 등 수정 쿼리는 즉시 반영되며 되돌릴 수 없습니다.
- JOIN / GROUP BY / 서브쿼리 등 고급 SQL은 지원하지 않습니다.
- 문자열 비교 시 작은따옴표(' ')로 감싸야 합니다.

