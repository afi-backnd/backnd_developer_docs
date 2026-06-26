---
sidebar_label: 데이터 모델링
---

# 데이터 모델링

데이터베이스를 사용하기 위해서는 테이블과 매핑될 C# 클래스를 정의해야 합니다.
제공되는 속성(Attribute)을 사용하여 테이블 이름, 컬럼 타입, 기본 키 등을 설정할 수 있습니다.

## 기본 구조

모든 데이터 모델 클래스는 `BaseModel`을 상속받아야 하며, `[Table]` 속성을 가져야 합니다.

```csharp
using BACKND.Database;

[Table("table_name")]
public class MyTable : BaseModel
{
    // 컬럼 정의
}
```

:::tip 콘솔에서 모델 코드 복사하기
데이터 모델링이 익숙하지 않다면, 콘솔에서 테이블을 생성한 후 자동 생성된 C# 모델 코드를 복사하여 사용할 수 있습니다. [[콘솔 가이드]](/guide/console-guide/database/table-editor/#데이터-수정)
:::

## 테이블 타입

데이터베이스는 두 가지 테이블 타입을 제공합니다.

### 1. UserTable (유저 테이블)
*   **목적**: 유저별 데이터를 관리하기 위한 테이블입니다.
*   **주요 사용 사례**:
    *   **유저 데이터**: 개인 설정, 보유 아이템, 작업 진행도 등 나만 볼 수 있는 정보
    *   **공개 프로필**: 닉네임, 레벨, 프로필 정보 등 다른 유저가 볼 수 있는 정보
    *   **경쟁 데이터**: 경쟁 점수, 랭킹 정보 등 다른 유저와 경쟁하는 정보
*   **특징**:
    *   `user_uuid` 컬럼이 자동으로 추가되어 데이터의 소유자를 식별합니다.
    *   기본적으로 자신의 데이터만 읽고 쓸 수 있도록 설정됩니다.
*   **권한**:
    *   `READ`: 기본값 `SELF` (자신만 읽기 가능)
    *   `WRITE`: 기본값 `SELF` (자신만 쓰기 가능)
    *   필요에 따라 `OTHERS` 권한을 추가하여 공개 프로필이나 랭킹 등을 구현할 수 있습니다.

### 2. FlexibleTable (자유 테이블)
*   **목적**: 메타 데이터, 시스템 설정, 로그 데이터 등 공용 데이터를 관리하기 위한 테이블입니다.
*   **주요 사용 사례**:
    *   **공유 이벤트**: 모든 유저가 공유하는 이벤트 상태 정보 (예: 월드 보스)
    *   **그룹 활동**: 그룹 간의 경쟁 현황 및 기록 (예: 길드 점령전)
    *   **로그 데이터**: 유저의 행동 로그, 결제 내역 등 분석을 위한 기록 데이터
*   **특징**:
    *   `user_uuid` 컬럼이 존재하지 않습니다.
    *   모든 유저가 접근할 수 있는 공용 데이터로 간주됩니다.
*   **권한**:
    *   `READ`: `OTHERS` 권한 포함 시 누구나 읽기 가능
    *   `WRITE`: `OTHERS` 권한 포함 시 누구나 쓰기 가능
    *   필요에 따라 `READ` 권한을 비워두고 `WRITE`만 허용하여 로그 데이터 기록 용도로 사용할 수 있습니다.

## 속성 (Attributes)

### [Table]
클래스가 매핑될 테이블 정보를 정의합니다.

| 파라미터 | 설명 | 예시 |
| :--- | :--- | :--- |
| name | 테이블 이름 (영문, 숫자, 언더스코어만 허용) | `"users"` |
| type | 테이블 타입 (`UserTable`, `FlexibleTable`) | `TableType.UserTable` |
| ClientAccess | 클라이언트 접근 허용 여부 | `true` |
| ReadPermissions | 읽기 권한 설정 (`SELF`, `OTHERS`) | `new[] { TablePermission.SELF }` |
| WritePermissions | 쓰기 권한 설정 (`SELF`, `OTHERS`) | `new[] { TablePermission.SELF }` |

### [Column]
테이블의 컬럼을 정의합니다.

| 파라미터 | 설명 |
| :--- | :--- |
| name | (선택) 컬럼 이름. 생략 시 변수명 사용 |
| type | (선택) 데이터 타입. 생략 시 자동 추론 |
| NotNull | `true`일 경우 null 값을 허용하지 않음 |
| Default | (선택) 기본값. SQL 함수 또는 리터럴 값을 지정 가능 |

#### 기본값과 SQL 함수

컬럼의 기본값으로 SQL 함수를 지정할 수 있습니다. 지원되는 SQL 함수는 다음과 같습니다.

| SQL 함수 | 설명 | 사용 예시 |
| :--- | :--- | :--- |
| `NOW()` | 현재 날짜와 시간을 반환 | `Default = "NOW()"` |
| `UUID()` | 고유한 UUID 값을 생성 | `Default = "UUID()"` |

**SQL 함수와 문자열 구분 규칙**

기본값으로 지정된 문자열이 SQL 함수로 인식되려면 정확히 함수 형태여야 합니다. 그렇지 않으면 일반 문자열로 처리됩니다.

| 입력값 | 변환 결과 | 설명 |
| :--- | :--- | :--- |
| `"NOW()"` | `NOW()` | SQL 함수로 인식 |
| `"UUID()"` | `UUID()` | SQL 함수로 인식 |
| `"hello"` | `'hello'` | 일반 문자열로 처리 |
| `"NOW() test"` | `'NOW() test'` | 일반 문자열로 처리 (정확한 함수 형태가 아님) |

```csharp
[Column(Default = "NOW()")]
public DateTime CreatedAt { get; set; }

[Column(Default = "UUID()")]
public string UniqueId { get; set; }

[Column(Default = "active")]  // 일반 문자열
public string Status { get; set; }
```

### [PrimaryKey]
테이블의 기본 키(Primary Key)를 지정합니다.

| 파라미터 | 설명 |
| :--- | :--- |
| AutoIncrement | `true`일 경우 값이 자동으로 증가 (테이블당 1개만 설정 가능) |

## 지원 데이터 타입

C# 데이터 타입은 다음과 같이 데이터베이스 타입으로 매핑됩니다.

| C# 타입 | 데이터베이스 타입 | 설명 |
| :--- | :--- | :--- |
| `int`, `long` | `int32`, `int64` | 정수형 데이터 |
| `uint`, `ulong` | `uint32`, `uint64` | 부호 없는 정수형 데이터 |
| `float`, `double` | `float` | 실수형 데이터 |
| `bool` | `bool` | 논리형 데이터 (true/false) |
| `string` | `string` | 문자열 (최대 4KB) |
| `DateTime` | `datetime` | 날짜 및 시간 (ISO 8601) |
| `List`, `Dictionary`, `Class` | `json` | 복합 데이터 (최대 12KB) |

### NULL 허용 컬럼

데이터베이스에서 NULL 값을 허용하려면 C#의 Nullable 타입(`?`)을 사용해야 합니다.

```csharp
[Column(NotNull = true)]
public int Score { get; set; }      // NOT NULL - NULL 값 저장 불가

[Column]
public int? Score { get; set; }     // NULL 허용 - NULL 값 저장 가능
```

**Nullable 타입 사용 규칙**

| C# 타입 | NULL 허용 | 설명 |
| :--- | :--- | :--- |
| `int`, `long`, `float`, `double`, `bool`, `DateTime` | ❌ | 값 타입은 기본적으로 NULL 불가 |
| `int?`, `long?`, `float?`, `double?`, `bool?`, `DateTime?` | ✅ | Nullable 값 타입은 NULL 허용 |
| `string`, `List`, `Dictionary`, `Class` | ✅ | 참조 타입은 기본적으로 NULL 허용 |

:::warning
`NotNull = true` 속성과 Nullable 타입을 함께 사용하면 `NotNull` 속성이 우선 적용됩니다.

```csharp
[Column(NotNull = true)]
public int? Score { get; set; }     // NotNull이 우선 → NULL 값 저장 불가
```
:::

## 명명 규칙

테이블 이름과 컬럼 이름은 다음 규칙을 따라야 합니다.

*   **길이**: 1 ~ 64자
*   **허용 문자**: 영문자(a-z, A-Z), 숫자(0-9), 언더스코어(_)
*   **시작 문자**: 영문자로 시작해야 함
*   **금지**: 특수문자, 공백, 예약어(SQL 키워드 등) 사용 불가
*   **대소문자**: 대소문자를 구분하지 않으며, 저장 시 소문자로 변환됩니다.

## 예제 코드

### 유저 테이블 (UserTable) 예시
자신의 정보만 수정할 수 있고, 다른 사람도 내 정보를 볼 수 있는(공개 프로필) 설정입니다.

```csharp
[Table("user_profile", TableType.UserTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.SELF, TablePermission.OTHERS }, WritePermissions = new[] { TablePermission.SELF })]
public class UserProfile : BaseModel
{
    [PrimaryKey(AutoIncrement = true)]
    [Column(NotNull = true)]
    public long Id { get; set; }

    [Column(NotNull = true)]
    public string Nickname { get; set; }

    [Column]
    public int? Level { get; set; } = 1;

    [Column]
    public DateTime? LastLogin { get; set; }
}
```

### 자유 테이블 (FlexibleTable) 예시
모든 유저가 읽을 수 있지만, 쓰기는 제한된(관리자 전용) 공지사항 테이블입니다.

```csharp
[Table("announcements", TableType.FlexibleTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.OTHERS }, WritePermissions = new[] { TablePermission.OTHERS })]
public class Announcement : BaseModel
{
    [PrimaryKey(AutoIncrement = true)]
    [Column(NotNull = true)]
    public int Id { get; set; }

    [Column(NotNull = true)]
    public string Title { get; set; }

    [Column]
    public string Content { get; set; }

    [Column]
    public DateTime? CreatedAt { get; set; }
}
```
