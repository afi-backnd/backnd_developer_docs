---
sidebar_label: 데이터 조회
description: "데이터 조회"
---

# 데이터 조회

LINQ를 사용하여 직관적이고 강력한 쿼리를 작성할 수 있습니다.

## 기본 조회

### 전체 목록 조회
`ToList()`를 사용하여 테이블의 모든 데이터를 조회합니다.

```csharp
var allUsers = await DBClient.From<User>().ToList();
```

### 단일 항목 조회
`FirstOrDefault()`를 사용하여 조건에 맞는 첫 번째 항목을 조회합니다.

```csharp
var user = await DBClient.From<User>()
    .Where(x => x.Username == "user_001")
    .FirstOrDefault();
```

## 조건 검색 (Where)

`Where` 절을 사용하여 다양한 조건을 조합할 수 있습니다.

:::warning 성능 주의
**인덱스(Index)**나 **기본 키(PrimaryKey)**를 사용하지 않는 조건 검색은 테이블 전체를 탐색하는 **풀 스캔(Full Scan)** 방식으로 처리됩니다.
데이터가 많아질수록 조회 속도가 느려질 수 있으므로, 빈번하게 조회하는 필드는 인덱스로 설정하거나 기본 키를 활용하는 것이 좋습니다. [인덱스 생성 가이드](/guide/console-guide/database/table-editor/#테이블-수정)
:::

```csharp
// 레벨이 10 이상인 유저
var highLevelUsers = await DBClient.From<User>()
    .Where(x => x.Level >= 10)
    .ToList();

// 경험치가 200 초과 1200 미만인 유저
var midUsers = await DBClient.From<User>()
    .Where(x => x.Experience > 200 && x.Experience < 1200)
    .ToList();

// 활성 상태이면서 레벨이 5 초과인 유저
var activeHighLevel = await DBClient.From<User>()
    .Where(x => x.IsActive == true && x.Level > 5)
    .ToList();

// 문자열 포함 검색 (Contains)
var usersWithPlayer = await DBClient.From<User>()
    .Where(x => x.Username.Contains("player"))
    .ToList();

// 날짜 비교
var recentUsers = await DBClient.From<User>()
    .Where(x => x.CreatedAt > DateTime.UtcNow.AddDays(-1))
    .ToList();
```

## 정렬 및 페이징

### 정렬 (OrderBy / OrderByDescending)

```csharp
// 레벨 내림차순 정렬 (높은 순)
var topUsers = await DBClient.From<User>()
    .OrderByDescending(x => x.Level)
    .ToList();

// 경험치 오름차순 정렬 (낮은 순)
var beginnerUsers = await DBClient.From<User>()
    .OrderBy(x => x.Experience)
    .ToList();
```

### 개수 제한 및 건너뛰기 (Take / Skip)

`Take`와 `Skip`을 조합하여 페이징 기능을 구현할 수 있습니다.

```csharp
// 상위 3명만 조회
var top3 = await DBClient.From<User>()
    .OrderByDescending(x => x.Level)
    .Take(3)
    .ToList();

// 2페이지 조회 (한 페이지당 2명씩일 때, 2명 건너뛰고 2명 조회)
var secondPage = await DBClient.From<User>()
    .OrderBy(x => x.Id)
    .Skip(2)
    .Take(2)
    .ToList();
```

## 집계 (Count / Any)

데이터의 개수를 세거나 존재 여부를 확인할 수 있습니다.

```csharp
// 전체 유저 수
long totalCount = await DBClient.From<User>().Count();

// 조건에 맞는 유저 수
long activeCount = await DBClient.From<User>()
    .Where(x => x.IsActive == true)
    .Count();

// 특정 조건의 데이터 존재 여부 확인
bool hasHighLevel = await DBClient.From<User>()
    .Where(x => x.Level > 10)
    .Any();
```
