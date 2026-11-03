---
sidebar_label: 뒤끝 데이터베이스 사용
description: "뒤끝 데이터베이스 사용"
---

# 뒤끝 데이터베이스 사용

뒤끝펑션 안에서 [뒤끝 데이터베이스](/sdk-docs/database/intro)를 읽고 쓸 수 있습니다. 유니티의 데이터베이스 SDK와 같은 `BACKND.Database` API를 사용합니다.  
서버에 배포된 함수는 테이블의 **클라이언트 접근 권한과 관계없이** 데이터를 읽고 쓸 수 있습니다. 그래서 아래 구성을 만들 수 있습니다.  

- 게임 클라이언트는 테이블을 읽기만 하고, 쓰기는 권한 설정으로 막습니다.  
- 재화 차감, 아이템 지급, 거래처럼 조작되면 안 되는 쓰기는 함수에서만 합니다.  

전체 흐름을 따라 해 보려면 [아이템 뽑기](/sdk-docs/function/tutorial/gacha), [거래소 만들기](/sdk-docs/function/tutorial/exchange) 튜토리얼을 차례로 진행해 주세요.  

## 준비 사항

- 뒤끝 콘솔에서 데이터베이스를 활성화하고 테이블을 만들어 두어야 합니다. 테이블을 만드는 방법은 [테이블 에디터](/guide/console-guide/database/table-editor) 문서를 참고해 주세요.  
- 데이터베이스 UUID가 필요합니다. 뒤끝 콘솔의 데이터베이스 [설정](/guide/console-guide/database/settings) 화면에서 복사합니다.  

## 1. 클라이언트 만들기

`Client`는 함수 클래스의 `static readonly` 필드로 한 번 만들어 재사용합니다.  
`Backend.Initialize(ref stream)`를 호출한 뒤에 사용합니다. 데이터베이스 API는 모두 `Task`를 반환하므로 함수를 `async Task<Stream>`으로 작성합니다.  

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Amazon.Lambda.Core;
using BackEnd;
using BACKND.Database;
using BACKND.Database.Exceptions;
using Newtonsoft.Json.Linq;

[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.Json.JsonSerializer))]

namespace BackendFunction
{
    public class BFunc
    {
        // 뒤끝 콘솔의 데이터베이스 설정 화면에서 복사한 UUID를 넣습니다.
        static readonly Client Db = new Client("00000000-0000-0000-0000-000000000000");

        public async Task<Stream> Function(Stream stream, ILambdaContext context)
        {
            try
            {
                Backend.Initialize(ref stream);
            }
            catch (Exception e)
            {
                return ReturnErrorObject("initialize " + e.Message);
            }

            // 여기부터 Db를 사용할 수 있습니다.
            return Backend.StringToStream("success");
        }

        static Stream ReturnErrorObject(string err)
        {
            JObject error = new JObject();
            error.Add("error", err);

            return Backend.StringToStream(error.ToString());
        }
    }
}
```

테이블 모델(`[Table]`, `[Column]`, `[PrimaryKey]`)은 유니티 데이터베이스 SDK와 같은 방법으로 정의합니다. [데이터 모델링](/sdk-docs/database/data-modeling) 문서를 참고해 주세요. 콘솔에서 만든 테이블과 테이블 이름, 컬럼 이름, 타입이 같아야 합니다.  

## 2. 실행 환경에 따라 달라지는 권한

같은 코드라도 어디서 실행하느냐에 따라 테이블 권한이 다르게 적용됩니다. 코드로 바꿀 수 없습니다.  

| 실행 환경 | 테이블의 클라이언트 접근 권한 |
| --- | --- |
| 서버에 배포한 함수(`backend invoke`, 유니티에서 호출) | 적용되지 않습니다. 모든 테이블을 읽고 쓸 수 있습니다. |
| 내 PC의 `backend debug`(`--user` 포함), 편집기의 F5 실행 | 게임 클라이언트와 같이 적용됩니다. |

:::caution 쓰기를 막은 테이블은 내 PC에서 시험할 수 없습니다
클라이언트 쓰기를 막은 테이블에 쓰는 코드는 내 PC에서 실행하면 권한 오류(`FailureKind`가 `Permission`)로 실패합니다.  
이런 코드는 개발용 프로젝트에 배포한 뒤 [`backend invoke`](/sdk-docs/function/cli/invoke)나 유니티에서 호출해 확인합니다. 내 PC에서는 요청 값 확인과 읽기처럼 권한이 허용된 부분만 확인할 수 있습니다.  
:::

## 3. 클라이언트 쓰기 막기

테이블의 클라이언트 접근 권한은 뒤끝 콘솔의 [테이블 에디터](/guide/console-guide/database/table-editor#클라이언트-접근-권한)에서 테이블을 만들거나 수정할 때 정합니다.  
함수에서만 쓰게 하려면 쓰기 권한을 모두 해제합니다.  

| 테이블 | 용도 | 읽기 | 쓰기 |
| --- | --- | --- | --- |
| 유저 테이블 | 지갑, 인벤토리처럼 본인만 보는 데이터 | 본인 | 모두 해제 |
| 자유 테이블 | 판매 목록처럼 모두가 보는 데이터 | 허용 | 모두 해제 |
| 자유 테이블 | 정산 기록처럼 함수만 다루는 데이터 | 해제 | 해제 |

:::caution 자유 테이블은 본인과 타인을 구분하지 않습니다
자유 테이블에는 행의 소유자가 없습니다. 쓰기 권한을 하나라도 남겨 두면 모든 클라이언트가 모든 행을 쓸 수 있습니다.  
자유 테이블의 쓰기를 막으려면 쓰기 권한을 모두 해제해 주세요.  
:::

모델에서도 같은 권한을 적어 두면, 코드로 테이블을 만들 때(`Db.CreateTable<T>()`) 같은 설정이 적용됩니다. 쓰기 권한을 모두 해제하려면 빈 배열을 넣습니다.  

```csharp
[Table("wallet", TableType.UserTable,
    ReadPermissions = new[] { TablePermission.SELF },
    WritePermissions = new TablePermission[0])]
public class Wallet : BaseModel
{
    [PrimaryKey]
    [Column]
    public string OwnerId { get; set; } = string.Empty;

    [Column(NotNull = true)]
    public long Gold { get; set; }
}
```

이미 만든 테이블의 권한은 모델을 바꿔도 바뀌지 않습니다. 콘솔의 테이블 수정에서 바꿔 주세요.  

## 4. 호출한 사용자와 다른 사용자의 데이터

| 테이블 | 함수에서 다룰 수 있는 행 |
| --- | --- |
| 유저 테이블 | **함수를 호출한 사용자 본인의 행만** 다룰 수 있습니다. 배포한 함수에서도 같습니다. 쿼리마다 `OfCurrentUser()`를 붙여야 합니다. |
| 자유 테이블 | 모든 행을 다룰 수 있습니다. `OfCurrentUser()`는 쓸 수 없습니다. |

```csharp
// 유저 테이블: 호출한 사용자의 지갑
var wallet = await Db.From<Wallet>()
    .OfCurrentUser()
    .FirstOrDefault();
```

유저 테이블의 `user_uuid` 컬럼은 예약 컬럼이라 모델이나 `Where`에 쓸 수 없습니다. 따라서 함수에서 다른 사용자의 유저 테이블 행은 읽거나 바꿀 수 없습니다.  
거래 대금처럼 다른 사용자에게 가는 값은 아래와 같이 처리합니다.  

1. 자유 테이블에 받을 사람을 나타내는 컬럼(예: `SellerId`)을 두고 기록합니다.  
2. 받을 사람이 함수를 호출하면, 그 사람의 gamerId와 기록의 `SellerId`가 같은지 확인한 뒤 본인의 유저 테이블에 반영합니다.  

호출한 사용자의 gamerId를 얻는 방법은 [호출한 사용자 확인하기](/sdk-docs/function/request-response/caller) 문서를 참고해 주세요. `OfCurrentUser()`도 같은 사용자를 대상으로 합니다.  

:::danger 권한 판단에 요청 값을 쓰지 마세요
배포한 함수는 테이블 권한을 받지 않으므로, 어떤 행을 바꿀지는 함수 코드가 결정합니다.  
`Backend.Content`로 받은 사용자 ID를 `Where` 조건에 그대로 넣으면 클라이언트가 다른 사람의 행을 바꿀 수 있습니다. 소유자 확인에는 서버가 확인한 gamerId를 사용합니다.  
:::

## 5. 트랜잭션으로 함께 바꾸기

재화 차감과 아이템 지급처럼 함께 바뀌어야 하는 쓰기는 트랜잭션으로 묶습니다. 트랜잭션은 전부 반영되거나 전부 취소됩니다.  

```csharp
// gamerId: 호출한 사용자의 gamerId(GetUserInfoV2로 확인한 값)
// InventoryItem: ItemId(기본 키), ItemCode 컬럼을 가진 유저 테이블 모델
long price = 500;
string itemId = Guid.NewGuid().ToString();

var result = await Db.Transaction()
    // 골드가 price 이상일 때만 차감합니다. 모자라면 트랜잭션 전체가 취소됩니다.
    .From<Wallet>().OfCurrentUser()
        .Where(x => x.OwnerId == gamerId && x.Gold >= price)
        .Dec(x => x.Gold, price)
        .Update()
    .From<InventoryItem>().OfCurrentUser()
        .Insert(new InventoryItem { ItemId = itemId, ItemCode = "sword" })
    .Commit();

if (result.Success == false)
{
    return ReturnErrorObject("purchase failed: " + result.Error);
}
```

트랜잭션 규칙은 아래와 같습니다.  

- 한 트랜잭션에 작업을 최대 10개까지 넣을 수 있고, `Insert`, `Update`, `Delete`만 넣을 수 있습니다. 읽기는 트랜잭션 밖에서 합니다.  
- `Update`와 `Delete`는 `Where`에 기본 키를 `==`로 지정해 한 행을 정합니다. 함께 적은 나머지 조건(`x.Gold >= price` 등)은 실행 조건이 됩니다.  
- 실행 조건이 하나라도 맞지 않거나 대상 행이 없으면 트랜잭션 전체가 취소됩니다. 이미 있는 기본 키로 `Insert`해도 전체가 취소됩니다.  
- 한 트랜잭션에서 같은 행을 두 번 다룰 수 없습니다.  
- `Commit()`은 실패해도 예외를 던지지 않습니다. 반드시 `result.Success`를 확인하고, 실패 이유는 `result.Error`에서 확인합니다.  

## 6. 같은 요청이 두 번 반영되지 않게 하기

데이터베이스 SDK는 쓰기를 자동으로 다시 보내지 않습니다. 하지만 유니티가 응답을 받지 못해 같은 요청을 다시 보내거나, 쓰기 요청이 시간 초과로 끝났는데 실제로는 반영된 경우가 있습니다.  
같은 요청이 다시 와도 결과가 두 번 반영되지 않도록 설계합니다.  

- 상태 값을 실행 조건으로 겁니다. 예: 판매 목록을 `Status == "open"`일 때만 `"sold"`로 바꾸면 두 번째 구매는 조건이 맞지 않아 취소됩니다.  
- 한 번만 생겨야 하는 기록은 기본 키를 정해 `Insert`합니다. 예: 정산 기록의 기본 키를 판매 목록 ID로 두면 같은 판매의 정산은 한 번만 생깁니다.  
- 결과가 매번 달라지는 쓰기(뽑기처럼 난수를 쓰는 경우)는 클라이언트가 만든 요청 ID를 기록의 기본 키로 저장합니다. 같은 요청 ID가 다시 오면 새로 처리하지 않고 저장해 둔 결과를 돌려줍니다. 예: [아이템 뽑기](/sdk-docs/function/tutorial/gacha)  
- 함수 안에서 읽은 값은 다른 요청 때문에 바로 바뀌었을 수 있습니다. 읽은 값(가격, 상태)을 트랜잭션의 실행 조건에 다시 넣어 확인합니다.  

## 7. 오류 처리하기

트랜잭션이 아닌 쿼리는 실패하면 `BACKND.Database.Exceptions.DatabaseException`을 던집니다. `FailureKind`로 원인을 구분합니다.  

```csharp
try
{
    var items = await Db.From<InventoryItem>().OfCurrentUser().ToList();
}
catch (DatabaseException e)
{
    return ReturnErrorObject("database " + e.FailureKind + ": " + e.Message);
}
```

| `FailureKind` | 의미 |
| --- | --- |
| `Permission` | 테이블 권한으로 거부되었습니다. 내 PC에서 쓰기를 막은 테이블에 쓴 경우입니다. |
| `Validation` | 쿼리나 값이 올바르지 않습니다. 모델과 콘솔의 테이블 정의가 같은지, `Where` 식이 지원되는지 확인합니다. |
| `Authentication` | 사용자를 확인하지 못했습니다. `Backend.Initialize`를 먼저 호출했는지, 로그인한 사용자로 실행했는지 확인합니다. |
| `Conflict` | 다른 데이터와 충돌했습니다. |
| `RateLimit` | 요청이 너무 많습니다. |
| `UnknownWriteOutcome` | 쓰기 요청이 서버에 도착했는지 알 수 없습니다. 실제로 반영됐을 수 있으므로, 다시 쓰기 전에 데이터를 읽어 확인합니다. |

## 8. Where 식 작성 규칙

- 비교하는 값은 상수나 지역 변수만 사용할 수 있습니다. `listing.Price`처럼 객체의 프로퍼티를 직접 쓰면 실행할 때 거부되므로 지역 변수에 담아 사용합니다.  

  ```csharp
  long price = listing.Price;  // 지역 변수에 담습니다.
  var query = Db.From<MarketListing>().Where(x => x.Price == price);
  ```

- 문자열은 `==`와 `!=`로만 비교할 수 있습니다. 상태 값은 `"open"`, `"sold"`처럼 문자열로 두면 다루기 쉽습니다.  
- 지원하는 연산자는 [지원 연산자](/sdk-docs/database/supported-operators) 문서를 참고해 주세요.  

## 응답 시간

데이터베이스 요청은 함수의 응답 시간에 그대로 더해지고, 함수는 정해진 시간 안에 끝나야 합니다. 자세한 내용은 [타임아웃](/sdk-docs/function/basic/timeout) 문서를 참고해 주세요.  
한 번 호출할 때 필요한 읽기만 하고, 쓰기는 트랜잭션 하나로 묶어 요청 수를 줄입니다.  
