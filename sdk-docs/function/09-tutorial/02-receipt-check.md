---
sidebar_label: 영수증 검증 및 아이템 지급
description: "영수증 검증 및 아이템 지급"
---

# 영수증 검증 및 아이템 지급

유니티에서 Unity IAP 5.x(Unity Purchasing)로 결제한 뒤 받은 영수증을 뒤끝펑션으로 보내고, **뒤끝펑션에서 영수증을 검증한 뒤 아이템을 지급하는 방법**을 다루는 튜토리얼입니다.  

## 준비 사항

- [에코 함수](/sdk-docs/function/tutorial/echo-function) 튜토리얼처럼 프로젝트를 만들고 배포하는 방법을 알고 있어야 합니다.
- 유니티 프로젝트에 뒤끝 SDK와 Unity Purchasing 5.0 이상이 설치되어 있어야 합니다. 원스토어로 출시한다면 [원스토어 IAP SDK(v21)](https://onestore-dev.gitbook.io/dev/tools/billing/v21/unity)도 필요합니다.
- 뒤끝 콘솔에 스토어별 영수증 검증 설정이 되어 있어야 합니다. [구글 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/android/setting-google-console), [애플 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/ios/setting-google-console), [원스토어 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/onestore/setting-onestore-console)
- 뒤끝 콘솔의 게임 정보 관리에 아이템을 저장할 `item` 테이블이 있어야 합니다.

## 1. 인증 정보 등록하기

이 튜토리얼의 함수는 영수증 검증과 게임 정보 저장 같은 뒤끝 기능을 호출합니다. 뒤끝펑션 인증키와 함께 클라이언트 앱 ID, 시그니처 키, 패키지 이름을 등록하고 테스트 계정으로 로그인해 둡니다. 이미 등록했다면 건너뜁니다.  

```powershell
backend config
```

질문마다 값을 붙여 넣고, `Log in a test user ...? [y/N]` 질문에 `y`를 입력해 테스트 계정 ID와 비밀번호로 로그인합니다.  
아래와 같이 세 항목이 모두 표시되면 준비된 것입니다. 질문의 자세한 내용은 [인증 정보 등록](/sdk-docs/function/cli/setting)을 참고해 주세요.  

```text
Saved profile 'default'.
  deploy:       configured
  SDK identity: configured
  test user:    active (te****01), renewed automatically
```

## 2. 프로젝트 만들기

```powershell
mkdir C:\work -Force
cd C:\work
backend new giveSword
cd giveSword
```

프로젝트 이름 `giveSword`가 서버에 배포될 함수 이름이 됩니다.  

## 3. 함수 작성하기

`Function.cs`의 내용을 모두 지우고 아래 코드로 바꿉니다.  
클라이언트가 보낸 스토어 타입(`google`, `apple`, `onestore`)과 영수증으로 [영수증 검증](/sdk-docs/function/backnd-features/receipt-check/validate-receipt)을 호출하고, 검증에 성공하면 `item` 테이블에 검 아이템을 추가합니다.  
뒤끝펑션은 함수를 호출한 기기의 플랫폼을 알 수 없으므로, 스토어 타입을 함수에서 직접 지정합니다.  
원스토어는 결제 금액과 통화를 함께 받고, 상품 ID는 함수에 고정한 값으로 검증합니다. `ONESTORE_SWORD_PRODUCT_ID`를 원스토어 개발자센터에 등록한 검 상품의 ID로 바꿔 주세요.  

```csharp
using System;
using System.IO;
using Amazon.Lambda.Core;
using BackEnd;
using Newtonsoft.Json.Linq;

[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.Json.JsonSerializer))]

namespace BackendFunction
{
    public class BFunc
    {
        // 원스토어에 등록한 검 상품의 ID입니다. 원스토어 영수증은 이 상품의 영수증만 검증에 성공합니다.
        const string ONESTORE_SWORD_PRODUCT_ID = "sword_pack";

        // 원스토어 글로벌로 출시했다면 true로 바꿉니다.
        const bool ONESTORE_IS_GLOBAL = false;

        public Stream Function(Stream stream, ILambdaContext context)
        {
            try
            {
                // 뒤끝펑션 SDK를 초기화합니다.
                Backend.Initialize(ref stream);
            }
            catch (Exception e)
            {
                return ReturnErrorObject("initialize " + e.Message);
            }

            // 스토어 타입과 영수증이 있는지 확인합니다.
            if (Backend.HasKey("store") == false || Backend.HasKey("receipt") == false)
            {
                return ReturnErrorObject("store or receipt key is not exist");
            }

            string store = Backend.Content["store"].ToString();
            string receipt = Backend.Content["receipt"].ToString();

            // 뒤끝펑션은 함수를 호출한 기기의 플랫폼을 알 수 없으므로 스토어 타입을 직접 지정합니다.
            ReceiptParam receiptParam;

            if (store == "google" || store == "apple")
            {
                receiptParam = new ReceiptParam(store, receipt);
            }
            else if (store == "onestore")
            {
                // 원스토어는 서버가 금액을 조회할 수 없으므로 결제 금액과 통화를 함께 받습니다.
                if (Backend.HasKey("price") == false || Backend.HasKey("currency") == false)
                {
                    return ReturnErrorObject("price or currency key is not exist");
                }

                // 소수점을 ,(쉼표)로 쓰는 국가의 금액은 .으로 바꿔야 0으로 처리되지 않습니다.
                string price = Backend.Content["price"].ToString().Replace(",", ".");
                string currency = Backend.Content["currency"].ToString();

                // 상품 ID는 클라이언트가 보낸 값이 아니라 함수에 고정한 값을 씁니다.
                receiptParam = new ReceiptParam("onestore", receipt)
                    .SetProductId(ONESTORE_SWORD_PRODUCT_ID)
                    .SetIsGlobal(ONESTORE_IS_GLOBAL)
                    .SetIapPrice(price, currency);
            }
            else
            {
                return ReturnErrorObject("unsupported store: " + store);
            }

            // 지급할 아이템을 영수증 검증 내역에 함께 기록합니다.
            receiptParam.AddDetailItemId("sword_0").AddDetailItemName("검");

            BackendReturnObject bro = Backend.Receipt.ValidateReceipt(receiptParam);

            // 영수증 검증에 실패하면 오류 코드와 이유를 반환합니다.
            // 이미 검증한 영수증이면 UsedReceipt, 환불되거나 취소된 영수증이면 AbnormalReceipt입니다.
            if (bro.IsSuccess() == false)
            {
                return ReturnErrorObject("fail to validate receipt: " + bro.GetMessage(), bro.GetErrorCode());
            }

            // 지급할 아이템 정보를 만듭니다.
            Param param = new Param();
            param.Add("name", "sword_0");
            param.Add("atk", 999);

            // item 테이블에 아이템을 추가합니다.
            bro = Backend.GameData.Insert("item", param);
            if (bro.IsSuccess() == false)
            {
                return ReturnErrorObject("fail to insert item: " + bro.GetMessage(), bro.GetErrorCode());
            }

            JObject result = new JObject();
            result.Add("ok", true);
            result.Add("item", "sword_0");

            return Backend.StringToStream(result.ToString());
        }

        static Stream ReturnErrorObject(string err, string errorCode = null)
        {
            JObject error = new JObject();
            error.Add("error", err);

            if (errorCode != null)
            {
                error.Add("errorCode", errorCode);
            }

            return Backend.StringToStream(error.ToString());
        }
    }
}
```

## 4. 내 PC에서 확인하기

영수증 검증에는 실제 결제에서 받은 영수증이 필요하므로, 내 PC에서는 전체 흐름을 실행할 수 없습니다.  
먼저 인증 정보 없이 실행해 요청 값 확인이 동작하는지 봅니다.  

`requests/default.json`의 내용을 아래와 같이 바꾸고 실행합니다. 이 파일의 내용이 함수에 전달됩니다.  

```json
{}
```

```powershell
backend debug
```

결과의 `execution.result`에 `{"error": "store or receipt key is not exist"}`가 표시되면 정상입니다.  

아이템 지급이 정상적으로 되는지 확인하려면, `store`와 `receipt` 확인부터 영수증 검증까지의 코드를 임시로 건너뛰도록 바꾼 뒤 1단계에서 로그인한 [테스트 계정으로 실행](/sdk-docs/function/cli/debug#테스트-계정으로-실행하기)합니다. 요청이 `{}`이면 `store`와 `receipt` 확인에서 먼저 끝나기 때문입니다.  

```powershell
backend debug --user
```

확인한 뒤에는 반드시 원래 코드로 되돌리고 배포해 주세요.  

:::caution 실제 데이터가 바뀝니다
`--user`로 실행하면 실제 서버의 `item` 테이블에 아이템이 추가됩니다.  
테스트 계정과 테스트용 프로젝트에서 실행해 주세요.  
:::

## 5. 서버에 배포하기

1단계에서 등록한 뒤끝펑션 인증키로 배포합니다.  

```powershell
backend deploy
```

확인 질문에 함수 이름 `giveSword`가 표시되면 `y`를 입력합니다.  
배포가 끝나면 뒤끝 콘솔의 뒤끝펑션 메뉴에서 `giveSword`를 확인합니다.  

## 6. 유니티에서 호출하기

Google Play와 App Store는 Unity IAP 5.x로, 원스토어는 원스토어 IAP SDK로 결제를 받습니다. 두 경우 모두 결제가 끝나면 영수증을 뒤끝펑션으로 보내고, 함수가 아이템을 지급했을 때만 구매를 끝냅니다.  

### Google Play와 App Store (Unity IAP 5.x)

Unity IAP 5.x에서는 결제가 끝나 지급을 기다리는 구매가 `OnPurchasePending`으로 전달됩니다.  
여기서 영수증을 뒤끝펑션으로 보내고, 함수가 아이템을 지급하면 `ConfirmPurchase`로 구매를 확정합니다. 확정하지 않은 구매는 앱을 다시 시작했을 때 `OnPurchasePending`으로 다시 전달됩니다.  
상품 조회(`FetchProducts`)와 구매 요청(`PurchaseProduct`)을 포함한 전체 흐름은 뒤끝베이스의 [Unity Purchasing 연동 예제](/sdk-docs/backend/base/receipt/unity-iap5-example)를 참고해 주세요.  

```csharp
using BackEnd;
using UnityEngine;
using UnityEngine.Purchasing;

public class GiveSwordPurchase : MonoBehaviour
{
    StoreController storeController;

    async void Start()
    {
        // 구매 이벤트를 먼저 등록한 뒤 스토어에 연결합니다.
        storeController = UnityIAPServices.StoreController();
        storeController.OnPurchasePending += OnPurchasePending;
        await storeController.Connect();

        // 상품 조회와 구매 요청은 Unity Purchasing 연동 예제를 참고해 추가합니다.
    }

    // 결제가 끝나 지급을 기다리는 구매가 전달됩니다.
    void OnPurchasePending(PendingOrder order)
    {
        Param param = new Param();

        if (Application.platform == RuntimePlatform.IPhonePlayer || Application.platform == RuntimePlatform.OSXPlayer)
        {
            // iOS는 StoreKit 2의 JWS 영수증을 보냅니다.
            param.Add("store", "apple");
            param.Add("receipt", order.Info.Apple.jwsRepresentation);
        }
        else
        {
            // Android는 Unity IAP가 준 영수증을 그대로 보냅니다.
            param.Add("store", "google");
            param.Add("receipt", order.Info.Receipt);
        }

        Backend.BFunc.InvokeFunction("giveSword", param, callback =>
        {
            // 확정하지 않은 구매는 앱을 다시 시작했을 때 다시 전달됩니다.
            if (IsGrantedOrUsed(callback))
            {
                storeController.ConfirmPurchase(order);
            }
        });
    }

    // 검을 지급했거나(ok가 true) 이미 검증한 영수증(UsedReceipt)이면 true를 반환합니다.
    // 그 밖의 결과는 모두 실패로 보고 구매를 끝내지 않습니다.
    public static bool IsGrantedOrUsed(BackendReturnObject callback)
    {
        if (callback.IsSuccess() == false)
        {
            Debug.LogError("뒤끝펑션 실행 실패: " + callback);
            return false;
        }

        // 함수가 돌려준 JSON은 result 아래에 있습니다.
        var result = callback.GetReturnValuetoJSON()["result"];

        bool granted = result.IsObject && result.ContainsKey("ok") && result["ok"].IsBoolean && (bool)result["ok"];
        bool used = result.IsObject && result.ContainsKey("errorCode") && result["errorCode"].ToString() == "UsedReceipt";

        if (granted || used)
        {
            return true;
        }

        Debug.LogError("영수증 검증 실패: " + result.ToJson());
        return false;
    }
}
```

성공하면 `callback`의 `result`에 함수가 돌려준 `{"ok": true, "item": "sword_0"}`이 들어 있습니다.  
구매는 `ok`가 `true`이거나 `errorCode`가 `UsedReceipt`일 때만 확정하고, 그 밖의 결과는 모두 실패로 처리합니다.  

### 원스토어 (원스토어 IAP SDK)

원스토어 IAP SDK에서는 결제가 끝난 구매가 `IPurchaseCallback.OnPurchaseSucceeded`로 전달됩니다.  
결제 토큰과 함께 `QueryProductDetails`로 받아 둔 상품의 금액과 통화를 뒤끝펑션으로 보내고, 함수가 아이템을 지급하면 `ConsumePurchase`로 구매를 소비합니다.  
3일 안에 소비하지 않은 구매는 자동으로 환불되므로, 앱을 시작할 때 `QueryPurchases`를 호출해 소비하지 않은 구매를 다시 받아 같은 방법으로 처리합니다. `QueryPurchases`로 받은 구매도 `OnPurchaseSucceeded`로 전달됩니다.  
`PurchaseClientImpl` 초기화, 상품 조회, 구매 요청은 뒤끝베이스의 [원스토어 영수증 검증](/sdk-docs/backend/base/receipt/receipt/onestore/verify) 문서와 [원스토어 IAP SDK 문서](https://onestore-dev.gitbook.io/dev/tools/billing/v21/unity)를 참고해 주세요.  

아래는 `IPurchaseCallback`을 구현한 클래스에 넣는 구매 처리 부분입니다. `purchaseClient`는 초기화한 `PurchaseClientImpl`입니다.  

```csharp
// QueryProductDetails로 받은 상품 정보입니다. 결제 금액과 통화를 함수에 함께 보냅니다.
List<ProductDetail> productDetails = new List<ProductDetail>();

public void OnProductDetailsSucceeded(List<ProductDetail> details)
{
    productDetails = details;
}

// 결제가 끝난 구매와 QueryPurchases로 다시 받은 구매가 전달됩니다.
public void OnPurchaseSucceeded(List<PurchaseData> purchases)
{
    foreach (PurchaseData purchase in purchases)
    {
        ProductDetail detail = productDetails.Find(p => p.productId == purchase.ProductId);

        if (detail == null)
        {
            Debug.LogError("상품 정보가 없습니다: " + purchase.ProductId);
            continue;
        }

        Param param = new Param();
        param.Add("store", "onestore");
        param.Add("receipt", purchase.PurchaseToken);
        param.Add("price", detail.price);
        param.Add("currency", detail.priceCurrencyCode);

        Backend.BFunc.InvokeFunction("giveSword", param, callback =>
        {
            // 결과 판정은 Unity IAP 코드와 같습니다. 소비하지 않은 구매는 QueryPurchases로 다시 받습니다.
            if (GiveSwordPurchase.IsGrantedOrUsed(callback))
            {
                purchaseClient.ConsumePurchase(purchase);
            }
        });
    }
}
```

:::caution 키 이름과 함수 이름을 맞춰 주세요
유니티에서 보내는 키 이름(`store`, `receipt`, `price`, `currency`)은 함수에서 확인하는 키 이름과 대소문자까지 같아야 합니다.  
`InvokeFunction`의 함수 이름도 배포한 함수 이름(`giveSword`)과 같아야 합니다.  
:::

:::info 검증 뒤 지급 전에 실패한 구매
영수증 검증은 성공했지만 아이템을 지급하기 전에 함수가 실패하면, 같은 구매를 다시 보냈을 때 `UsedReceipt`가 반환됩니다. 이 튜토리얼은 이때 다시 지급하지 않고 구매만 확정합니다.  
이런 구매까지 다시 지급하려면 [UsedReceipt 응답 상세](/sdk-docs/function/backnd-features/receipt-check/validate-receipt#usedreceipt-응답-상세)를 참고해 영수증별 지급 여부를 따로 기록해 주세요.  
:::

:::caution 영수증의 상품을 확인해 주세요
이 튜토리얼의 함수는 Google Play와 App Store 영수증이 유효하면 어떤 상품을 결제했는지와 관계없이 검을 지급합니다. 판매하는 상품이 하나뿐인 게임을 가정한 예제입니다.  
상품이 여러 개라면 같은 게임의 다른 상품(예: 더 싼 상품)의 영수증으로도 검이 지급됩니다. 실제 서비스에서는 검증한 영수증의 상품 ID가 지급할 아이템의 상품 ID와 같은지 확인한 뒤 지급해 주세요.  
원스토어는 함수에 고정한 상품 ID(`ONESTORE_SWORD_PRODUCT_ID`)로 검증하므로, 다른 상품의 영수증은 검증에 실패합니다(`NoSuchData`).  
:::
