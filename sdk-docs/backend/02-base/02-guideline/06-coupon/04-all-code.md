---
description: "전체 코드"
---

# 전체 코드

## BackendCoupon.cs
```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendCoupon
{
    private static BackendCoupon _instance = null;

    public static BackendCoupon Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendCoupon();
            }

            return _instance;
        }
    }

    public void CouponUse(string couponNumber)
    {
        var bro = Backend.Coupon.UseCoupon(couponNumber);

        if (bro.IsSuccess() == false)
        {
            Debug.LogError("쿠폰 사용 중 에러가 발생했습니다. : " + bro);
            return;
        }

        Debug.Log("쿠폰 사용에 성공했습니다. : " + bro);

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

        foreach (LitJson.JsonData item in bro.GetFlattenJSON()["itemObject"])
        {
            if (item["item"].ContainsKey("itemType"))
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

        BackendGameData.Instance.GameDataUpdate();
    }
}
```

## BackendManager.cs
```js
using UnityEngine;


// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendManager : MonoBehaviour
{
    void Start()
    {
        var bro = Backend.Initialize(); // 뒤끝 초기화

        // 뒤끝 초기화에 대한 응답값
        if (bro.IsSuccess())
        {
            Debug.Log("초기화 성공 : " + bro); // 성공일 경우 statusCode 204 Success
        }
        else
        {
            Debug.LogError("초기화 실패 : " + bro); // 실패일 경우 statusCode 400대 에러 발생 
        }

        Test();
    }

    // 동기 함수를 비동기에서 호출하게 해주는 함수(유니티 UI 접근 불가)
    void Test()
    {
        BackendLogin.Instance.CustomLogin("user1", "1234");

        BackendCoupon.Instance.CouponUse("f9ffeed2c882bc1418"); // [추가] 쿠폰 코드가 couponNumber인 쿠폰 사용

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/game-log/before">다음  챕터로</a>
</div>
