---
sidebar_label: 원스토어 영수증 검증
---

# OneStore.ValidateReceipt
public Task< ValidateReceiptResult > **ValidateReceiptAsync**(bool **isGlobal**, string **productId**, string **purchaseToken**, string **description**);  
public Task< ValidateReceiptResult > **ValidateReceiptAsync**(bool **isGlobal**, string **productId**, string **purchaseToken**, string **description**, string **price**, string **currency**);


:::danger 영수증 검증 이용 시 주의 사항
**Unity IAP 5.X 버전 미지원 안내**   
Unity IAP 5.x 버전이 정식 출시 되었으나 내부구조가 기존과 상이하여 뒤끝의 업데이트 지원이 필요합니다.  
**뒤끝의 Unity IAP 5.X 버전 공식 지원 전까지는 영수증 검증 기능 이용 시 Unity IAP 4.13.0을 사용해 주시기 바랍니다.**  

**일부 국가의 iap Price 데이터 치환 사용 안내**   
화폐 숫자 표현에서 `.`(소수점)을 `,`(쉼표)로 사용하는 일부 국가들이 존재합니다.  
이러한 국가에서는 각 스토어에서 제공하는 iapPrice 값도 `,`가 포함된 형태로 전달됩니다.  
뒤끝의 영수증 검증은 숫자 데이터만 처리할 수 있기 때문에, `,`가 포함된 값을 iapPrice 파라미터로 그대로 사용할 경우 금액 정보가 정상적으로 반영되지 않고 0으로 처리됩니다.  
**`,` 가 포함된 iapPrice 데이터는 `.`로 치환하여 사용해 주세요.**  
:::

### 사용 불가능한 특수문자
| 특수문자 | 한글명 |
|---|---|
| ' | 작은 따옴표 |
| \ | 역슬래시 |

### 사용 가능한 특수문자
| 특수문자 | 한글명 |
|---|---|
| . | 마침표 |
| , | 쉼표 |
| " | 큰따옴표 |
| () |  괄호 |
| !  | 느낌표 |
| ? | 물음표 |
| ~ | 물결 |
| @ | 골뱅이 |
| *  | 곱하기 |
| + | 더하기 |
| - | 빼기 |
| / | 슬래시 |

**표기가 되지 않은 특수문자를 사용할 경우에는 영수증 검증이 정상적으로 진행되는지 확인 후 적용해주세요.**  

## 파라미터

| Value|  Type | Description |
| --- | --- | --|
| isGlobal | bool | 결제가 국외에서 발생했을 경우 true, 국내에서 발생했을 경우 false |
| productId | string | OneStore.Purchasing.PurchaseData.purchase.ProductId |
| purchaseToken | string | OneStore.Purchasing.PurchaseData.purchase.PurchaseToken |
| description | string | 추가로 저장하고자 하는 내용 |
| price | string | 해당 아이템의 가격 |
| currency | string | 해당 아이템의 통화 |

## 설명
[원스토어에서 지원하는 인앱 결제 서비스](https://onestore-dev.gitbook.io/dev/tools/tools)의 IPurchaseCallback.OnConsumeSucceeded()에서 구매한 상품에 대한 영수증을 받아 뒤끝 서버를 통해 영수증 검증을 받을 수 있습니다.  
* 뒤끝은 **영수증 자체의 유효성과, 구매한 productId**를 검증합니다.  
* 뒤끝 로그인 없이 뒤끝 영수증 검증 기능을 사용하는 것은 불가능합니다.  

### 금액 표시(선택 사항)
뒤끝 콘솔의 영수증 검증 항목에서 해당 구매 내역의 금액을 표시하고자 할 경우, 인자값 price와 currency를 추가해야합니다.

PurchaseClientImpl클래스의 QueryProductDetails 호출 시, 등록된 IPurchaseCallback의 핸들러에서 OnProductDetailsSucceeded가 호출되며 금액을 확인할 수 있습니다.


```js
public class YourCallback: IPurchaseCallback
{
    public void OnProductDetailsSucceeded(List<ProductDetail> productDetails)
    {
        OneStoreReceiptTest.productDetailList = productDetails;
        Debug.Log("OnProductDetailsSucceeded ");

        foreach (var pro in productDetails)
        {
            Debug.Log($"{pro.title} / {pro.type} / {pro.price} / {pro.promotionPrice} / {pro.productId}");
        }
    }

    public void OnPurchaseSucceeded(List<PurchaseData> purchases)
    {
        Debug.Log("OnPurchaseSucceeded ");

        foreach (var pro in purchases)
        {
            OneStoreReceiptTest.purchaseClient.ConsumePurchase(pro);
        }
    }

    public async void OnConsumeSucceeded(PurchaseData purchase)
    {
        Debug.Log("OnConsumeSucceeded ");

        string productPrice = "";
        string productCurrency = "";
        OneStoreReceiptTest.productDetailList.ForEach(productDetail =>
        {
            if (productDetail.productId == purchase.ProductId)
            {
                productPrice = productDetail.price;
                productCurrency = productDetail.priceCurrencyCode;
            }
        });

        var reqResult = await BackndIAP.OneStore.ValidateReceiptAsync(false, purchase.ProductId, purchase.PurchaseToken, "description", productPrice, productCurrency);
        if (reqResult.IsSuccess())
        {
            Debug.Log("ValidateOneStoreReceipt : " + reqResult);
        }
        else
        {
            Debug.LogError("ValidateOneStoreReceipt : " + reqResult);
        }
    }
}


public class OldOneStoreReceiptTest : MonoBehavior
{
    // Start is called before the first frame update
    public static PurchaseClientImpl purchaseClient = null;

    public static List<ProductDetail> productDetailList = new List<ProductDetail>();
    public static List<string> productList = new List<string>();

    public void InitializeOneStore()
    {
        purchaseClient = new PurchaseClientImpl("MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQ~~");
        purchaseClient.Initialize(new YourCallback());
        purchaseClient.LaunchUpdateOrInstallFlow(action =>
        {
            Debug.Log("LaunchUpdateOrInstallFlow " + action.ToString());
        }); 
        
        productList.Add("onestore_gamepoint");
        purchaseClient.QueryProductDetails(productList.AsReadOnly(), ProductType.ALL);
    }

    private void Purchase()
    {
        if (purchaseClient == null)
        {
            Debug.LogError("purchaseClient is null");
            return;
        }
        
        Debug.Log("원스토어 로그인 성공");

        var builder = new PurchaseFlowParams.Builder();
        builder.SetProductId(productList[0]);
     
        purchaseClient.Purchase(builder.Build());

        new OneStoreAuthClientImpl().LaunchSignInFlow(signInResult =>
        {
            if (signInResult.IsSuccessful())
            {
                Debug.Log("LaunchSignInFlow Success");
            }
            else
            {
                Debug.LogError("LaunchSignInFlow Fail");

            }
        });
    }    
}
```

## Example

### Task 방식

```js
List<ProductDetail> productDetailList;

public void BuyButton(int i)
{
    string productId = "gold";

    // 결제 로직
    ProductType productType = ProductType.INAPP;
    var purchaseFlowParams = new PurchaseFlowParams.Builder()
        .SetProductId(productId)                // 추가 필수
        .SetProductType(productType)            // 추가 필수
        .SetDeveloperPayload("따로 메모할만한 정보 여기에 넣기")  // 선택사항
        .Build();
    purchaseClient.Purchase(purchaseFlowParams);
}

public async void OnPurchaseSucceeded(List<PurchaseData> purchases)
{
    bool isGlobal = false; // 원스토어에 국내에서만 앱을 출시하였을 경우

    /*
    뒤끝 영수증 검증 처리
    */
    for (int i = 0; i < purchases.Count; i++)
    {
        var validation = await BackndIAP.OneStore.ValidateReceiptAsync(isGlobal, purchases[i].ProductId, purchases[i].PurchaseToken, "receiptDescription");
        // 영수증 검증에 성공한 경우
        if (validation.IsSuccess())
        {
            Debug.Log($"ProcessPurchase: PASS. Product: {purchases[i].ProductId}");

            if (purchases[i].ProductId == "gold")
            {
                // 골드 지급
            }

        }
        else
        {
            Debug.Log($"ProcessPurchase: FAIL. Unrecognized product: {purchases[i].ProductId}");
        }
    }
}
```

### Callback 방식

```js
public void BuyButton(int i)
{
    string productId = "gold";

    // 결제 로직
    ProductType productType = ProductType.INAPP;
    var purchaseFlowParams = new PurchaseFlowParams.Builder()
        .SetProductId(productId)                // 추가 필수
        .SetProductType(productType)            // 추가 필수
        .SetDeveloperPayload("따로 메모할만한 정보 여기에 넣기")  // 선택사항
        .Build();
    purchaseClient.Purchase(purchaseFlowParams);
}

public void OnPurchaseSucceeded(List<PurchaseData> purchases)
{
    bool isGlobal = false; // 원스토어에 국내에서만 앱을 출시하였을 경우
    /*
    뒤끝 영수증 검증 처리
    */
    for (int i = 0; i < purchases.Count; i++)
    {
        BackndIAP.OneStore.ValidateReceipt
            (isGlobal, purchases[i].ProductId, purchases[i].PurchaseToken, "receiptDescription", (callback) =>
        {
            // 영수증 검증에 성공한 경우
            if (callback.IsSuccess())
            {
                Debug.Log($"ProcessPurchase: PASS. Product: {purchases[i].ProductId}");

                if (purchases[i].ProductId == "gold")
                {
                    // 골드 지급
                }

            }
            else
            {
                Debug.Log($"ProcessPurchase: FAIL. Unrecognized product: {purchases[i].ProductId}");
            }
        });
    }
}
```


## ReturnCase

### Success cases

**성공한 경우**  
statusCode : 201  
message : Success  
returnValue : **{"usedDate":"2018-10-15T05:17:49Z"}**  

### Error cases


**콘솔에 원스토어 정보가 올바르지 않는 경우**  
statusCode : 400  
errorCode : UndefinedParameterException  
message : undefined onestore client_id, onestore client_id을(를) 확인할 수 없습니다

**영수증 검증이 유효하지 않을 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다

**영수증 토큰이 string.Empty일 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : undefined token, token을(를) 확인할 수 없습니다

**productId가 string.Empty일 경우**  
statusCode : 400  
errorCode : BadParameterException  
message : undefined productId, productId을(를) 확인할 수 없습니다

**위변조된 영수증 토큰**  
statusCode : 400  
errorCode : BadParameterException  
message : bad token, 잘못된 token 입니다

**이미 사용한 영수증 토큰**  
statusCode : 409  
errorCode : UsedReceipt  
message : This receipt has already been used. usedDate: **2018-02-15T04:01:50.000Z**
