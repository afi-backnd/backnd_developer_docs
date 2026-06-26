---
description: "활용 : 프로젝트간 쿠폰 발급 공유"
---

# 활용 : 프로젝트간 쿠폰 발급 공유

**데이터베이스 기능에는 자유 테이블 기능이 제공되어, 유저에게 귀속되지 않은 데이터를 생성할 수 있습니다.**  
이를 활용하여 사전 생성한 쿠폰 데이터를 삽입하고, 조건에 맞는 유저에게 쿠폰을 할당해줄 수 있습니다.

## 테이블 생성 및 쿠폰 코드 등록

![테이블 생성 화면](/img/docs/faq/database/faq/image-01.png)

- **coupon 테이블**
  - 자유 테이블
  - 클라이언트 접근 권한은 모두 부여
- **coupon_no**
  - 쿠폰 발급에 따른 순번을 확인하기 위한 컬럼 (테이블 생성 시 기본 `id` 컬럼을 컬럼명만 적절히 변경하여 처리)
- **coupon_code**
  - `string`, 기본값 비워두기, Nullable 체크
  - B 프로젝트에서 실제 발급된 쿠폰 코드
- **gamer_uuid**
  - `string`, 기본값 비워두기, Nullable 체크
  - 유저에게 쿠폰 할당 시 해당 유저의 UUID를 등록할 컬럼
- **인덱스 컬럼 구성**
  - `gamer_uuid`와 `coupon_no`를 인덱스 컬럼 조합으로 처리  
    (`coupon_no`는 이미 기본키 컬럼이기에 `gamer_uuid`만 등록하여도 무방)
  - **인덱스(Index)** 나 **기본 키(Primary Key)** 를 사용하지 않는 조건 검색은 테이블 전체를 탐색하는 **풀 스캔(Full Scan)** 방식으로 처리됩니다.  
    데이터가 많아질수록 조회 속도가 느려질 수 있으므로, 빈번하게 조회하는 필드는 인덱스로 설정하거나 기본 키를 활용하는 것이 좋습니다.
  - 단, 인덱스 컬럼으로 설정 시 해당 컬럼은 데이터가 저장된 이후 `null` 값으로의 재수정은 불가하니 꼭 참고 부탁드립니다.  
    (비어있던 인덱스 컬럼에 데이터가 삽입된 후에는 다른 데이터로의 변경 저장은 가능하지만 `null` 값으로는 저장 불가)

데이터 삽입 기능을 통해 사전 생성된 쿠폰 코드를 업로드하여 데이터를 구성합니다.

![쿠폰 데이터 업로드 화면 1](/img/docs/faq/database/faq/image-02.png)

![쿠폰 데이터 업로드 화면 2](/img/docs/faq/database/faq/image-03.png)

## 쿠폰 발급을 위한 예제 코드

- 동시성, 쿠폰 소진 등 여러 상황을 고려하여 예시 코드를 구성하였습니다.  
  조금 더 효율적인 코드(자신에게 발급된 쿠폰도 재조회하지 않고, 미발급 쿠폰도 재조회 없이 남은 9개 쿠폰에서 재선택하는 등)도 충분히 구성 가능하기에 예시로써 참고해 주시면 감사하겠습니다.
- 예시로서의 코드이기에 실제 게임 운영 상황에 맞추어 이용해 주시기 바랍니다.
- 전체적인 흐름은 아래 플로우차트 이미지를 참고 바랍니다.

![플로우차트 1](/img/docs/faq/database/faq/image-04.png)

![플로우차트 2](/img/docs/faq/database/faq/image-05.png)

```csharp
// 테이블과 매핑될 C# 클래스 정의 (상단 이미지 참고, 콘솔에서 자동 구성된 내용 복사 가능)
[Table("coupon", TableType.FlexibleTable)]
public class Coupon : BaseModel
{
    [PrimaryKey(AutoIncrement = true)]
    [Column("coupon_no", DatabaseType.Int32, NotNull = true)]
    public int CouponNo { get; set; }

    [Column("coupon_code", DatabaseType.String)]
    public string CouponCode { get; set; }

    [Column("gamer_uuid", DatabaseType.String)]
    public string GamerUuid { get; set; }
}

public async void GetCoupon() {
    string myGameId = GetMyGameId();
    await GetOrAssignCouponAsync(myGameId);
}

// uuid는 세션 동안 1회만 조회
private string GetMyGameId() {
    BackendReturnObject bro = Backend.BMember.GetUserInfoV2();
    return bro.GetReturnValuetoJSON()["row"]["gamerId"].ToString();
}

private async Task GetOrAssignCouponAsync(string myGameId, int retry = 0) {
    if (retry >= 5) {
        Debug.LogError("쿠폰 발급 재시도 초과");
        return;
    }
    try {
        Debug.Log($"내 GameId: {myGameId}");

        // 1. 이미 발급된 쿠폰 확인
        var existingCoupon = await DBClient.From<Coupon>()
            .Where(c => c.GamerUuid == myGameId)
            .FirstOrDefault();

        if (existingCoupon != null) {
            Debug.Log($"이미 발급받은 쿠폰: {existingCoupon.CouponCode}");
            return;
        }

        Debug.Log("발급받은 쿠폰이 없음. 새로 발급 시도...");

        // 2. 미발급된 쿠폰 조회
        var emptyCoupons = await DBClient.From<Coupon>()
            .Where(c => c.GamerUuid == null)
            .Take(10)
            .ToList();

        Debug.Log($"비어있는 쿠폰 개수: {emptyCoupons?.Count ?? 0}");

        if (emptyCoupons != null && emptyCoupons.Count > 0) {
            int randomIndex = UnityEngine.Random.Range(0, emptyCoupons.Count);
            var selectedCoupon = emptyCoupons[randomIndex];

            Debug.Log($"선택된 쿠폰: No={selectedCoupon.CouponNo}, Code={selectedCoupon.CouponCode}");

            string couponCode = selectedCoupon.CouponCode;
            int couponNo = selectedCoupon.CouponNo;

            // 3. 쿠폰 업데이트 - 동시성 안전하게
            selectedCoupon.GamerUuid = myGameId;

            var result = await DBClient.From<Coupon>()
                .Where(c => c.CouponNo == couponNo && c.GamerUuid == null) // ★ null 조건 추가
                .Update(selectedCoupon);

            Debug.Log($"업데이트 결과: AffectedRows={result.AffectedRows}");

            if (result.AffectedRows > 0) {
                Debug.Log($"쿠폰 발급 성공! 쿠폰코드: {couponCode}");
            } else {
                Debug.LogWarning("다른 유저가 선점, 재시도...");
                await GetOrAssignCouponAsync(myGameId, retry + 1);
            }
        } else {
            Debug.LogWarning("사용 가능한 쿠폰이 없습니다.");
        }
    } catch (Exception e) {
        Debug.LogError($"쿠폰 처리 에러: {e.Message}");
    }
}
```

### 쿠폰 발급 케이스

![쿠폰 발급 케이스](/img/docs/faq/database/faq/image-06.png)

### 이미 발급된 케이스

유저에게 쿠폰 번호를 재노출하는 UI를 구성하도록 처리하면 됩니다.

![이미 발급된 케이스](/img/docs/faq/database/faq/image-07.png)

### 동시성 요청에 의한 다른 유저 선점 시 반복 및 실패 케이스

![동시성 실패 케이스](/img/docs/faq/database/faq/image-08.png)
