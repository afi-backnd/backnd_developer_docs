---
sidebar_label: 트랜잭션
---

# 트랜잭션

트랜잭션은 여러 데이터베이스 작업을 **하나의 원자적(Atomic) 단위**로 묶어 실행하는 기능입니다. 트랜잭션 내의 모든 작업은 **전부 성공하거나 전부 실패**합니다.

## 언제 사용하나요?

- **재화 거래**: 골드 차감과 아이템 지급이 동시에 이루어져야 할 때
- **인벤토리 교환**: 두 유저 간 아이템 교환 시 양쪽 모두 반영되어야 할 때

:::tip 핵심 포인트
트랜잭션을 사용하면 중간에 오류가 발생해도 **데이터 정합성**이 보장됩니다. 예를 들어, 골드를 차감한 후 아이템 지급에 실패하면 골드 차감도 자동으로 취소됩니다.
:::

## 기본 사용법

`Transaction()` 메서드로 트랜잭션 빌더를 생성하고, 작업을 추가한 후 `Commit()`로 실행합니다.

```csharp
var result = await DBClient.Transaction()
    .From<User>().Where(x => x.Id == visitorId).Update(updatedUser)
    .From<Inventory>().Insert(newItem)
    .Commit();

if (result.Success)
{
    Debug.Log($"트랜잭션 성공: {result.OperationCount}개 작업 완료");
}
else
{
    Debug.LogError($"트랜잭션 실패: {result.Error}");
}
```

## 지원 작업

| 작업 | 설명 |
|------|------|
| `Insert(model)` | 새 데이터 삽입 |
| `Update(model)` | 기존 데이터 수정 |
| `Delete()` | 조건에 맞는 데이터 삭제 |
| `Inc(field, value)` | 숫자 필드 증가 |
| `Dec(field, value)` | 숫자 필드 감소 |

## 사용 예제

### 재화 거래 (골드로 아이템 구매)

유저의 골드를 차감하고 인벤토리에 아이템을 추가하는 작업을 원자적으로 처리합니다.

```csharp
public async Task<bool> PurchaseItem(string visitorId, string itemCode, long price)
{
    var newItem = new InventoryItem
    {
        ItemCode = itemCode,
        Quantity = 1,
        AcquiredAt = DateTime.UtcNow
    };

    var result = await DBClient.Transaction()
        // 1. 골드 차감
        .From<UserCurrency>()
            .Where(x => x.UserId == visitorId)
            .Dec(x => x.Gold, price)
        // 2. 아이템 지급
        .From<InventoryItem>()
            .Insert(newItem)
        .Commit();

    return result.Success;
}
```

### 아이템 교환

두 유저 간 아이템을 안전하게 교환합니다.

```csharp
public async Task<bool> TradeItems(long myItemId, string myUserId, long theirItemId, string theirUserId, string myItemCode, string theirItemCode)
{
    var result = await DBClient.Transaction()
        // 1. 내 아이템 삭제
        .From<InventoryItem>()
            .Where(x => x.Id == myItemId)
            .Delete()
        // 2. 상대 아이템 삭제
        .From<InventoryItem>()
            .Where(x => x.Id == theirItemId)
            .Delete()
        // 3. 상대 아이템을 내게 지급
        .From<InventoryItem>()
            .Insert(new InventoryItem
            {
                ItemCode = theirItemCode,
                Quantity = 1,
                AcquiredAt = DateTime.UtcNow
            })
        // 4. 내 아이템을 상대에게 지급
        .From<InventoryItem>()
            .Insert(new InventoryItem
            {
                ItemCode = myItemCode,
                Quantity = 1,
                AcquiredAt = DateTime.UtcNow
            })
        .Commit();

    return result.Success;
}
```

## TransactionResult

`Commit()`는 `TransactionResult` 객체를 반환합니다.

| 속성 | 타입 | 설명 |
|------|------|------|
| `Success` | `bool` | 트랜잭션 성공 여부 |
| `OperationCount` | `int` | 트랜잭션에 포함된 작업 수 |
| `TotalAffectedRows` | `int` | 영향받은 총 행 수 |
| `Message` | `string` | 결과 메시지 |
| `Error` | `string` | 에러 메시지 (실패 시) |

```csharp
var result = await DBClient.Transaction()
    .From<User>().Where(x => x.Id == 1).Update(user)
    .From<Inventory>().Insert(item)
    .Commit();

if (result.Success)
{
    Debug.Log($"성공! {result.OperationCount}개 작업, {result.TotalAffectedRows}행 변경");
}
else
{
    Debug.LogError($"실패: {result.Error}");
    // 모든 작업이 자동으로 롤백됨
}
```

## OfCurrentUser()

`UserTable`에서 현재 로그인한 유저의 데이터만 대상으로 작업할 때 사용합니다.

```csharp
var result = await DBClient.Transaction()
    .From<UserCurrency>()
        .OfCurrentUser()
        .Dec(x => x.Gold, 1000)
    .From<InventoryItem>()
        .Insert(newItem)
    .Commit();
```

## 제한사항

:::warning 트랜잭션 제한
- **최대 작업 수**: 트랜잭션당 최대 **10개** 작업까지 지원됩니다.
- **지원 작업**: INSERT, UPDATE, DELETE만 지원됩니다. SELECT는 트랜잭션에 포함할 수 없습니다.
- **WHERE 조건**:
  - `Delete()`: 반드시 `Where()` 조건이 필요합니다.
  - `Update()`: `Where()`가 없으면 모델의 PrimaryKey로 자동 생성됩니다.
  - `Inc()`, `Dec()`: 반드시 `Where()` 조건이 필요합니다.
- **동일 필드 중복 수정 불가**: 같은 작업 내에서 동일한 필드를 여러 번 수정할 수 없습니다.
:::

```csharp
// 잘못된 예: 동일 필드 중복 수정
await DBClient.Transaction()
    .From<User>()
        .Where(x => x.Id == visitorId)
        .Inc(x => x.Gold, 100)
        .Inc(x => x.Gold, 50)  // 오류 발생
    .Commit();

// 올바른 예: 한 번에 합산하여 수정
await DBClient.Transaction()
    .From<User>()
        .Where(x => x.Id == visitorId)
        .Inc(x => x.Gold, 150)  // 100 + 50
    .Commit();
```
