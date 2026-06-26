---
sidebar_label: 지원하는 연산자
description: "지원하는 연산자"
---

# 지원하는 연산자

데이터베이스 LINQ 쿼리에서 사용할 수 있는 연산자 목록입니다.

## 왜 일부 연산자만 지원하나요?

데이터베이스 쿼리는 **성능과 안정성**을 위해 빠르게 처리 가능한 조회를 위해 엄선된 연산자만 지원합니다.   
일부 복잡한 연산자는 큰 부하를 줄 수 있어 제한됩니다.

:::warning 서비스 안정성을 위한 제한
**지원하지 않는 기능:**
- 닉네임에 "드래곤" 포함 검색 → ❌ 과부하 위험
- JSON 아이템 데이터 내부 검색 → ❌ 너무 느림

**대신 이렇게 설계하세요:**
- 검색할 값은 별도 컬럼으로 저장 (예: 유저 등급, 아이템 타입)
- 완전히 일치하는 값으로 조회 (예: `UserType == "VIP"`)
:::

## 비교 연산자

기본적인 비교 연산자를 모두 지원합니다.

| 연산자 | 설명 | 예시 |
| :--- | :--- | :--- |
| `==` | 같음 | `Where(x => x.Level == 10)` |
| `!=` | 같지 않음 | `Where(x => x.Level != 10)` |
| `>` | 큼 | `Where(x => x.Experience > 1000)` |
| `>=` | 크거나 같음 | `Where(x => x.Level >= 5)` |
| `<` | 작음 | `Where(x => x.Level < 50)` |
| `<=` | 작거나 같음 | `Where(x => x.Level <= 100)` |

## 논리 연산자

여러 조건을 조합할 때 사용합니다.

| 연산자 | 설명 | 예시 |
| :--- | :--- | :--- |
| `&&` | AND (그리고) | `Where(x => x.Level >= 10 && x.IsActive)` |
| `\|\|` | OR (또는) | `Where(x => x.Level >= 10 \|\| x.Experience > 1000)` |
| `!` | NOT (부정) | `Where(x => !(x.Level == 10))` |

**복합 조건 사용 시 주의사항:**
```csharp
// 괄호로 우선순위 명확히 지정
var users = await DBClient.From<User>()
    .Where(x => (x.Level >= 10 || x.Experience > 1000) && x.IsActive)
    .ToList();
```

**NOT 연산자 사용 예시:**
```csharp
// !(조건)은 조건의 반대로 변환됩니다
var users = await DBClient.From<User>()
    .Where(x => !(x.Level == 10))  // x.Level != 10과 동일
    .ToList();

// 복합 조건의 부정
var users = await DBClient.From<User>()
    .Where(x => !(x.Level > 5))  // x.Level <= 5로 변환
    .ToList();

// 불리언 필드의 부정
var users = await DBClient.From<User>()
    .Where(x => !x.IsActive)  // IsActive가 false인 경우
    .ToList();
```

## 문자열 비교

문자열 필드는 `==`와 `!=` 연산자로만 비교할 수 있습니다.

```csharp
// ✅ 지원: 정확한 문자열 일치
var user = await DBClient.From<User>()
    .Where(x => x.Username == "player1")
    .FirstOrDefault();

// ✅ 지원: 문자열 불일치
var users = await DBClient.From<User>()
    .Where(x => x.Email != "admin@example.com")
    .ToList();
```

**부분 문자열 검색은 지원하지 않습니다:**

```csharp
// ❌ 미지원: Contains, StartsWith, EndsWith
var users = await DBClient.From<User>()
    .Where(x => x.Username.Contains("admin"))  // 에러 발생
    .ToList();

// ❌ 미지원: StartsWith
var users = await DBClient.From<User>()
    .Where(x => x.Username.StartsWith("guest_"))  // 미지원
    .ToList();

// ❌ 미지원: EndsWith
var users = await DBClient.From<User>()
    .Where(x => x.Email.EndsWith("@gmail.com"))  // 미지원
    .ToList();
```

:::warning 닉네임 부분 검색은 지원하지 않습니다
**❌ 불가능한 검색:**
```csharp
// "드래곤"이 포함된 닉네임 검색 - 에러 발생
var users = await DBClient.From<User>()
    .Where(x => x.Nickname.Contains("드래곤"))
    .ToList();
```
→ 유저 100만명 데이터를 하나씩 확인해야 해서 과부하 발생

**✅ 이렇게 설계하세요:**

**방법 1: 소규모 길드/친구 목록 (100명 이하)**
```csharp
// 전체 조회 후 클라이언트에서 검색
var guildMembers = await DBClient.From<GuildMember>()
    .Where(x => x.GuildId == myGuildId)  // 길드원만 조회
    .ToList();
var filtered = guildMembers.Where(x => x.Nickname.Contains("드래곤")).ToList();
```

**방법 2: 대규모 전체 유저 검색**
```csharp
// 카테고리를 별도 필드로 저장
public class User : BaseModel
{
    [Column] public string Nickname { get; set; }        // "드래곤킹"
    [Column] public string NicknameTag { get; set; }     // "드래곤" (검색용)
}

// 빠른 정확 검색
var users = await DBClient.From<User>()
    .Where(x => x.NicknameTag == "드래곤")
    .ToList();
```
:::

## 포함 연산자 (IN)

특정 값 목록에 포함되는 데이터를 조회할 때 사용합니다. 리스트의 `Contains` 메서드를 사용합니다.

```csharp
// 조회할 ID 목록 (User 모델의 Id 타입에 맞게 long 사용)
var targetIds = new List<long> { 1, 3, 5 };

// ID가 목록에 포함된 유저 조회 (SQL의 IN 절과 동일)
var users = await DBClient.From<User>()
    .Where(x => targetIds.Contains(x.Id))
    .ToList();
```

**실제 사용 예시:**
```csharp
// 특정 유저명 목록으로 조회
var usernames = new List<string> { "player1", "player2", "admin" };
var users = await DBClient.From<User>()
    .Where(x => usernames.Contains(x.Username))
    .ToList();
```

## 날짜/시간 연산

`DateTime` 타입에 대해서도 비교 연산자를 사용할 수 있습니다.

```csharp
// 최근 7일 이내 가입한 유저
var oneWeekAgo = DateTime.UtcNow.AddDays(-7);
var newUsers = await DBClient.From<User>()
    .Where(x => x.CreatedAt >= oneWeekAgo)
    .ToList();

// 최근 24시간 내 생성된 유저
var oneDayAgo = DateTime.UtcNow.AddDays(-1);
var recentUsers = await DBClient.From<User>()
    .Where(x => x.CreatedAt > oneDayAgo)
    .ToList();

// 특정 날짜 범위 조회
var startDate = new DateTime(2025, 1, 1);
var endDate = new DateTime(2025, 12, 31);
var users = await DBClient.From<User>()
    .Where(x => x.CreatedAt >= startDate && x.CreatedAt <= endDate)
    .ToList();
```

**DateTime 사용 시 권장사항:**
- 날짜 계산은 쿼리 실행 전에 변수에 할당하여 사용

## 주의사항 및 제약

### JSON 아이템 데이터 내부는 검색 불가

**❌ 불가능한 조회:**
```csharp
public class User : BaseModel
{
    [Column(DatabaseType.Json)]
    public List<Item> Inventory { get; set; }  // 인벤토리 아이템 목록
}

// 전설 등급 아이템을 가진 유저 검색 - 불가능!
var users = await DBClient.From<User>()
    .Where(x => x.Inventory.Any(item => item.Rarity == "Legendary"))
    .ToList();
```
→ JSON 내부는 검색할 수 없어서 에러 발생

**✅ 해결 방법**

**방법 1: 내 인벤토리만 확인 (1명)**
```csharp
// 내 데이터만 조회 후 클라이언트에서 확인
var myData = await DBClient.From<User>()
    .Where(x => x.UserId == myUserId)
    .FirstOrDefault();
    
var legendaryItems = myData.Inventory
    .Where(x => x.Rarity == "Legendary")
    .ToList();
```

**방법 2: 전체 유저 통계 (권장 설계)**
```csharp
// 중요한 통계는 별도 컬럼으로 저장
public class User : BaseModel
{
    [Column] public int? LegendaryItemCount { get; set; }  // 전설 아이템 개수
    
    [Column(DatabaseType.Json)]
    public List<Item> Inventory { get; set; }  // 상세 아이템 정보
}

// 빠른 조회 가능
var richUsers = await DBClient.From<User>()
    .Where(x => x.LegendaryItemCount > 0)
    .OrderByDescending(x => x.LegendaryItemCount)
    .Take(100)
    .ToList();
```

### 메서드 호출 제한
C# 내장 메서드는 쿼리로 변환되지 않으므로 사용할 수 없습니다.

```csharp
// ❌ 불가능: ToString(), Parse() 등 메서드 사용
var users = await DBClient.From<User>()
    .Where(x => x.Level.ToString() == "100")
    .ToList();

// ✅ 가능: 직접 비교
var users = await DBClient.From<User>()
    .Where(x => x.Level == 100)
    .ToList();
```

### 변수 사용 권장
쿼리 식 내부에서 복잡한 계산을 하기보다는, 값을 미리 변수에 할당하여 사용하는 것을 권장합니다.

```csharp
// ❌ 비권장: 쿼리 내에서 복잡한 계산
var users = await DBClient.From<User>()
    .Where(x => x.CreatedAt > DateTime.UtcNow.AddDays(-7).AddHours(-3))
    .ToList();

// ✅ 권장: 변수에 미리 할당
var targetDate = DateTime.UtcNow.AddDays(-7).AddHours(-3);
var users = await DBClient.From<User>()
    .Where(x => x.CreatedAt > targetDate)
    .ToList();
```

### 문자열 대소문자 구분
문자열 비교는 대소문자를 구분합니다.

```csharp
// "Player1"과 "player1"은 다른 값으로 취급됨
var user = await DBClient.From<User>()
    .Where(x => x.Username == "player1")  // 정확히 일치해야 함
    .FirstOrDefault();
```

:::tip 대소문자 무시 검색
대소문자를 무시하고 검색하려면 클라이언트 측에서 처리해야 합니다:
```csharp
var allUsers = await DBClient.From<User>().ToList();
var user = allUsers.FirstOrDefault(x => 
    x.Username.Equals("player1", StringComparison.OrdinalIgnoreCase));
```
:::

## 성능 최적화 팁

### 빠른 조회 vs 느린 조회

**✅ 빠른 조회 (추천)**
```csharp
// 레벨 50 이상 유저 조회 - 0.02초
var users = await DBClient.From<User>()
    .Where(x => x.Level >= 50)
    .ToList();

// 특정 길드 조회 - 0.01초
var members = await DBClient.From<User>()
    .Where(x => x.GuildId == 12345)
    .ToList();
```

**❌ 느린 조회 (피해야 함)**
```csharp
// 전체 유저 조회 후 필터링 - 5초+
var allUsers = await DBClient.From<User>().ToList();
var filtered = allUsers.Where(x => x.Level >= 50).ToList();
```

**📊 실제 서비스 상황 예시**

| 상황 | 나쁜 방법 (전체 조회) | 좋은 방법 (WHERE 사용) |
|:---|:---:|:---:|
| 길드원 100명 검색 | 0.5초 | 0.01초 |
| 친구 1,000명 검색 | 1초 | 0.02초 |
| 전체 유저 10만명 검색 | 5분+ (서비스 멈춤) | 0.03초 |
| 랭킹 100만명 정렬 | 10분+ (클라이언트 크래시 위험) | 0.05초 |

### 불필요한 데이터 조회 방지

전체 데이터를 조회한 후 필터링하면 **풀 스캔이 발생**합니다.

```csharp
// ❌ 나쁜 예: 풀 스캔 발생 (데이터 10만 건 → 10만 건 모두 전송)
var allUsers = await DBClient.From<User>().ToList();  // 전체 조회
var activeUsers = allUsers.Where(x => x.IsActive).ToList();   // 클라이언트 필터링
```

```csharp
// ✅ 좋은 예: 인덱스 기반 조회 (필요한 데이터만 전송)
var activeUsers = await DBClient.From<User>()
    .Where(x => x.IsActive)  // 서버에서 필터링
    .ToList();
```

:::danger 🚨 피해야 할 위험한 대용량 조회 코드
```csharp
// ❌ 절대 금지: 전체 유저 데이터 다운로드
var allUsers = await DBClient.From<User>().ToList();

// ❌ 절대 금지: 전체 전투 기록 다운로드
var allBattles = await DBClient.From<BattleLog>().ToList();
```

**왜 위험한가요?**
1. **클라이언트 크래시**: 데이터가 너무 많아서 메모리 부족
2. **엄청난 로딩**: 수십 초 이상 기다려야 함
3. **데이터 요금 폭탄**: 모바일에서 수백 MB 다운로드
4. **다른 유저도 느려짐**: 과부하 걸림

**✅ 올바른 방법:**
```csharp
// 내 정보만 조회
var myData = await DBClient.From<User>()
    .Where(x => x.UserId == myUserId)
    .FirstOrDefault();

// 필요한 범위만 조회 (상위 100명)
var topRankers = await DBClient.From<User>()
    .OrderByDescending(x => x.Score)
    .Take(100)
    .ToList();

// 조건에 맞는 데이터만 조회
var guildMembers = await DBClient.From<User>()
    .Where(x => x.GuildId == myGuildId)
    .ToList();
```
:::
