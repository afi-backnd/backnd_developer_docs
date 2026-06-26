---
sidebar_label: Step 3. 쿠폰 사용하고 아이템 저장하기
description: "Step 3. 쿠폰 사용하고 아이템 저장하기"
---



# 쿠폰 사용하고 아이템 저장하기

## 1. 쿠폰 사용 및 데이터 저장하기 함수 작성

사전 준비에서 작성한 [BackendCoupon.cs](/sdk-docs/backend/base/guideline/coupon/before)의 Coupon 함수에 내용을 추가합니다.  

### BackendCoupon.cs

**수정 전**  

```js
public async Task CouponUse(string couponNumber)
{
    // Step 3. 쿠폰 사용하고 아이템 저장하기
}
```

**수정 후**  

```js
public async Task CouponUse(string couponNumber)
{
    var reqResult = await BackndCoupon.Instance.UseCouponAsync(couponNumber);
    if (reqResult.IsSuccess() == false)
    {
        Debug.LogError("쿠폰 사용 중 에러가 발생했습니다. : " + reqResult);
        return;
    }

    Debug.Log("쿠폰 사용에 성공했습니다. : " + reqResult);

    if (BackendGameData.userData == null)
    {
        BackendGameData.Instance.GameDataGet();
    }

    if (BackendGameData.userData == null)
    {
        BackendGameData.Instance.GameDataInsert();
    }

    if (BackendGameData.userData == null)
    {
        Debug.LogError("userData가 존재하지 않습니다.");
        return;
    }        

    foreach (var item in reqResult.GetRows("itemObject"))
    {
        var itemObj = item["item"] as JObject;
        if (itemObj.ContainsKey("itemType"))
        {
            int itemId = int.Parse(item["item"]["itemId"].ToString());
            string itemType = item["item"]["itemType"].ToString();
            string itemName = item["item"]["itemName"].ToString();
            int itemCount = int.Parse(item["itemCount"].ToString());

            if (BackendGameData.userData.inventory.ContainsKey(itemName))
            {
                BackendGameData.userData.inventory[itemName] += itemCount;
            }
            else
            {
                BackendGameData.userData.inventory.Add(itemName, itemCount);
            }
        }
        else
        {
            Debug.LogError("지원하지 않는 item입니다.");
        }
    }

    await BackendGameData.Instance.GameDataUpdate();
}
```

## 2. 뒤끝 콘솔에서 쿠폰코드 복사

뒤끝 콘솔 `뒤끝 베이스 > 쿠폰  관리 `에서 생성한 쿠폰을 클릭하여 미사용된 쿠폰의 코드를 복사합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/coupon/coupon-console-couponcode-copy.png" />

## 3. BackendManager.cs에 함수 호출 추가

해당 함수가 호출되기 위해서는 게임 실행 시 자동으로 호출되는 BackendManager에서 호출해야 합니다.  
**뒤끝 초기화와 뒤끝 로그인**이 이루어진 후에 함수를 호출할 수 있도록 추가합니다.  

2번에서 복사한 쿠폰 코드는 CouponUse의 인자값으로 값을 붙여넣습니다.  

> 예시 : "쿠폰 코드" -> "f9ffeed2c882bc1418"

### BackendManager.cs

**수정 전**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234");

    // 쿠폰 사용 로직 추가

    Debug.Log("테스트를 종료합니다.");
}
```

**수정 후**  

```js
async void Test()
{
    await BackendLogin.Instance.CustomLogin("user1", "1234");

    // [변경 필요] 쿠폰 코드를 뒤끝 콘솔 > 쿠폰 관리 > 테스트 쿠폰에서 생성된 쿠폰코드 값으로 변경해주세요.  
    await BackendCoupon.Instance.CouponUse("쿠폰 코드"); // 예시 : "쿠폰 코드" -> "f9ffeed2c882bc1418"

    Debug.Log("테스트를 종료합니다.");
}
```

## 4. 유니티에서 테스트

스크립트를 수정한 후, 유니티 디버깅을 실행시키고 유니티의 Console 로그를 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/coupon/coupon-success-log.png" />

이때 로그에서 **'게임 정보 데이터 수정에 성공했습니다. : statusCode : 204'**가 표시되어야 함수 호출에 성공한 것입니다.  
해당 로그 외에 statusCode : 400, 404, 409 에러등이 발생할 경우에는 [UseCoupon에러케이스](/sdk-docs/backend/base/coupon/using)를 통해 어떠한 에러로 문제가 발생하였는지 확인할 수 있습니다.  

## 5. 콘솔에서 쿠폰 사용 여부 확인

쿠폰 사용이 성공된 경우에는 해당 쿠폰코드의 사용여부가 **사용**으로 변경됩니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/coupon/coupon-success-console-coupon.png" />

## 6. 게임 정보관리에서 데이터 변경 여부 확인

예제에서는 쿠폰 사용 후에 해당 지급된 아이템에 대한 데이터 저장도 실시하기에 데이터가 변경이 됩니다.  

`뒤끝베이스 > 게임 정보 관리` 에서 해당 테이블을 눌러, 데이터가 변경되었는지 확인합니다.  

<img src="https://developer.thebackend.io/static/img/outline/manual/beginner/coupon/coupon-success-console-gamedata.png" />

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/game-log/before">다음  챕터로</a>
</div>
