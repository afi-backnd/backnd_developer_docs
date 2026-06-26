


# 전체 코드

## BackendCoupon.cs
```js
using Newtonsoft.Json.Linq;
using System.Collections.Generic;
using System.Threading.Tasks;
using UnityEngine;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

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
}
```

## BackendManager.cs
```js
using UnityEngine;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class BackendManager : MonoBehaviour
{
    async void Start()
    {
        // 뒤끝 초기화
        var reqResult = await BackndBase.InitializeAsync();
        // 뒤끝 초기화에 대한 응답값
        if (reqResult.IsSuccess())
        {
            Debug.Log("초기화 성공 : " + reqResult); // 성공일 경우 statusCode 204 Success
        }
        else
        {
            Debug.LogError("초기화 실패 : " + reqResult); // 실패일 경우 statusCode 400대 에러 발생
        }

        Test();
    }
    
    async void Test()
    {
        await BackendLogin.Instance.CustomLogin("user1", "1234");

        await BackendCoupon.Instance.CouponUse("f9ffeed2c882bc1418"); // [추가] 쿠폰 코드가 couponNumber인 쿠폰 사용

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/game-log/before">다음  챕터로</a>
</div>
