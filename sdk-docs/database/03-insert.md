---
sidebar_label: 데이터 삽입
description: "데이터 삽입"
---

# 데이터 삽입

정의한 데이터 모델을 사용하여 데이터베이스에 새로운 데이터를 저장할 수 있습니다.

## 기본 삽입 (Insert)

`Insert` 메서드를 사용하여 데이터를 저장합니다. 작업은 비동기로 수행됩니다.

다음은 **길드 점령지(Territory)** 정보를 생성하는 예제입니다. 점령전 현황은 모든 유저가 공유해야 하므로 `FlexibleTable`을 사용하는 것이 적합합니다.

```csharp
[Table("territories", TableType.FlexibleTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.OTHERS }, WritePermissions = new[] { TablePermission.OTHERS })]
public class Territory : BaseModel
{
    [PrimaryKey]
    [Column(NotNull = true)]
    public string MapId { get; set; }

    [Column]
    public string Name { get; set; }

    [Column]
    public string OccupyingGuildName { get; set; }

    [Column(NotNull = true, Default = "NOW()")]
    public DateTime OccupiedAt { get; set; } = DateTime.Now;

    [Column(NotNull = true, Default = "0")]
    public int DefenseScore { get; set; }

    [Column(NotNull = true, Default = "false")]
    public bool IsDisputed { get; set; }
}

// 점령지 객체 생성
var territory = new Territory
{
    MapId = "map_001",
    Name = "북부 요새",
    OccupyingGuildName = "Legendary",
    OccupiedAt = DateTime.Now,
    DefenseScore = 5000,
    IsDisputed = false
};

// 데이터베이스에 삽입
var result = await DBClient.From<Territory>().Insert(territory);
Debug.Log($"삽입 완료: {result.AffectedRows}행 영향");
```

## InsertResult

`Insert()` 메서드는 `InsertResult` 객체를 반환합니다.

| 속성 | 타입 | 설명 |
|------|------|------|
| `AffectedRows` | `int` | 영향받은 행 수 |
| `LastInsertId` | `object` | 자동 증가된 PrimaryKey 값 (AUTO_INCREMENT 사용 시) |
| `Operation` | `string` | 실행된 작업 타입 ("INSERT") |
| `Message` | `string` | 결과 메시지 |

```csharp
var result = await DBClient.From<Territory>().Insert(territory);

if (result.AffectedRows > 0)
{
    Debug.Log($"삽입 성공! ID: {result.LastInsertId}");
}
```

## 복잡한 데이터 타입 삽입

리스트(List)나 딕셔너리(Dictionary), 커스텀 클래스 등의 복잡한 데이터 구조는 자동으로 JSON 형태로 직렬화되어 저장됩니다.
이때, 커스텀 클래스는 직렬화를 위해 `[Serializable]` 속성을 붙여야 할 수 있습니다.

### 커스텀 클래스 정의

```csharp
[Serializable]
public class ItemDetail
{
    public string Name { get; set; }
    public string Description { get; set; }
    public int AttackPower { get; set; }
    public bool IsMagic { get; set; }
}
```

### 데이터 삽입 예제

다음은 **거래소 아이템(TradeItem)** 을 등록하는 예제입니다. `List`와 `Dictionary`를 사용하여 태그와 속성을 저장하고, 위에서 정의한 `ItemDetail` 클래스를 필드로 포함하여 복잡한 데이터를 구조화하는 방법을 보여줍니다.

```csharp
[Table("trade_items", TableType.FlexibleTable, ClientAccess = true, ReadPermissions = new[] { TablePermission.OTHERS }, WritePermissions = new[] { TablePermission.OTHERS })]
public class TradeItem : BaseModel
{
    [PrimaryKey(AutoIncrement = true)]
    [Column(NotNull = true)]
    public long ItemId { get; set; }

    [Column(NotNull = true)]
    public string SellerId { get; set; }

    [Column(NotNull = true)]
    public int Price { get; set; }

    [Column(NotNull = true)]
    public DateTime RegisteredAt { get; set; }

    [Column]
    public List<string> Tags { get; set; }

    [Column]
    public Dictionary<string, int> Properties { get; set; }

    [Column]
    public ItemDetail ItemInfo { get; set; }
}

var tradeItem = new TradeItem
{
    SellerId = "User_001",
    Price = 1000,
    RegisteredAt = DateTime.UtcNow,

    Tags = new List<string> { "Weapon", "Legendary" },
    Properties = new Dictionary<string, int> { { "Durability", 100 } },

    ItemInfo = new ItemDetail
    {
        Name = "전설의 검",
        Description = "고대 영웅이 사용하던 검입니다.",
        AttackPower = 100,
        IsMagic = true
    }
};

await DBClient.From<TradeItem>().Insert(tradeItem);
```

## 대량 데이터 삽입

반복문을 사용하여 여러 데이터를 삽입할 수 있습니다. `Task.WhenAll`을 활용하면 병렬로 처리하여 속도를 높일 수 있습니다.

다음은 게임 플레이 중 발생하는 **로그(GameLog)** 데이터를 대량으로 저장하는 예제입니다.

```csharp
var insertTasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
    var log = new GameLog
    {
        ActionType = "MonsterKill",
        Message = $"Monster_{i} 처치",
        Timestamp = DateTime.UtcNow
    };

    // 작업을 리스트에 추가
    insertTasks.Add(DBClient.From<GameLog>().Insert(log));
}

// 모든 삽입 작업이 완료될 때까지 대기
await Task.WhenAll(insertTasks);
```
