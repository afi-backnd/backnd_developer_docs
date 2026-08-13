---
sidebar_label: "영수증 검증 예제"
description: "구글 영수증 검증 예제"
---

# 구글 영수증 검증 예제

뒤끝펑션에서 영수증 검증을 하기 위해서는 결제를 진행한 상품의 ID와 영수증 토큰이 필요합니다.  
유니티 IAP 기능을 활용하여 정상적으로 인앱결제를 완료했으면 아래 `ProcessPurchase` 함수가 호출됩니다.  

- 안드로이드 환경에서는 리턴된 영수증 토큰을 **위 파싱 함수를 이용하여 재 가공한 뒤 이를 뒤끝펑션으로 송신**해야 합니다.  
- iOS 환경에서는 리턴된 영수증 토큰을 **그대로 뒤끝펑션으로 송신**해야 합니다.  

```js
public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs args)
{
    string id = string.Empty;
    string token = string.Empty;
    Param param = new Param();

#if UNITY_IOS
    // ios의 경우 PurchaseEventArgs에 포함되어 있는 영수증 토큰을 그대로 뒤끝펑션으로 송신하면 됩니다.  
    param.Add("token", args.purchasedProduct.receipt);

    // ios 영수증에는 제품 ID가 포함되어 있지 않아
    // 만약 TBC를 충전하고자 한다면 productID를 구매 프로세스 진행 전 저장해두고, 이를 param에 포함해야 합니다.  
    param.Add("productID", productID);
    // 만약 TBC를 충전하고자 한다면 platform 정보를 포함해야 뒤끝펑션에서 TBC를 충전할 때 어떤 Platform 인지 확인할 수 있습니다.  
    param.Add("platform", "apple");

#elif UNITY_ANDROID
    // android의 경우 PurchaseEventArgs에 포함되어 있는 영수증 토큰을
    // BackEnd.Game.Payment.GoogleReceiptData.FromJson 함수를 이용하여 파싱하여 id와 token 값을 추출한 뒤
    // 이를 뒤끝펑션으로 송신해야 합니다.  
    BackEnd.Game.Payment.GoogleReceiptData.FromJson(args.purchasedProduct.receipt, out id, out token);
    param.Add("productID", id);
    param.Add("token", token);
    // 만약 TBC를 충전하고자 한다면 platform 정보를 포함해야 뒤끝펑션에서 TBC를 충전할 때 어떤 Platform 인지 확인할 수 있습니다.  
    param.Add("platform", "google");
#endif

    // 뒤끝펑션 호출
    Backend.BFunc.InvokeFunction("receiptVaildate", param, callback => {
        // TODO: 뒤끝펑션 진행 후 결과 확인
    });

    return PurchaseProcessingResult.Complete;
}
```
