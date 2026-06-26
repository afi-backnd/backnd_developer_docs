---
sidebar_label: 영수증 검증 및 아이템지급
description: "영수증 검증 및 아이템지급"
---

# 영수증 검증 및 아이템 지급

유니티에서 인앱결제를 진행한 후 생성된 영수증 토큰을 이용하여 **뒤끝펑션에서 영수증 검증을 한 후 아이템을 지급하는 방법**에 대한 튜토리얼입니다.  
튜토리얼을 정상적으로 진행하기 위해서는 뒤끝펑션 개발 툴과 뒤끝펑션 프로젝트 템플릿이 설치되어 있어야 합니다.  

---
####
1. BackendFunction 템플릿을 이용하여 새 프로젝트를 생성합니다.  

[프로젝트 생성 문서](/sdk-docs/function/work-in-windows/visual-studio-code/create)를 참고하여 새 프로젝트를 생성합니다.  

---
####
2. Function 함수를 아래와 같이 작성합니다.  

영수증 검증 함수의 경우 반드시 영수증 토큰이 필요하기 때문에 디버깅 시에는 별도의 로직을 작성하여
아이템 지급이 정상적으로 되는지만 확인할 수 있습니다.  

```js
public Stream Function(Stream stream, ILambdaContext context)
{
    try
    {
        // 뒤끝펑션 API 초기화
        Backend.Initialize(ref stream);
    }
    catch(Exception e)
    {
        // 뒤끝펑션 API 초기화를 실패한 경우
        return ReturnErrorObject("initialize " + e.ToString());
    }

    // debugConfig.json에서 `content` 혹은 뒤끝 SDK에서 InvokeFunction을 호출할 때 인자 값으로 넘긴 `Param` 에
    // token 키가 존재하는지 확인합니다.  
    if(Backend.HasKey("token") == false)
    {
      return ReturnErrorObject("token key is not exist");
    }

    BackendReturnObject bro = null;
    bool isApple = false;

    // Content에 productId 키가 존재하는지 확인합니다.  
    if(Backend.HasKey("productId") == false)
    {
      // 애플 영수증 검증에서는 productId가 필요하지 않다.  
      isApple = true;
    }

    // 안드로이드 영수증 검증인 경우
    if(isApple == false)
    {
      var pid = Backend.Content["productId"].ToString();
      var token = Backend.Content["token"].ToString();

      bro = Backend.Receipt.IsValidateGooglePurchase(pid, token, "뒤끝펑션에서 구글 영수증 검증");
    }
    // 애플 영수증 검증인 경우
    else
    {
      var token = Backend.Content["token"].ToString();

      bro = Backend.Receipt.IsValidateApplePurchase(token, "뒤끝펑션에서 애플 영수증 검증");
    }
    // 영수증 검증 결과 null인 경우 예외 처리
    if(bro == null)
    {
      return ReturnErrorObject("bro is null");
    }
    // 영수증 검증 결과 실패이면 실패 이유를 리턴
    if(bro.IsSuccess() == false)
    {
      return ReturnErrorObject("fail to vaildate receipt: " + bro.GetMessage());
    }

    // 지급할 아이템 정보 생성
    Param param = new Param();
    param.Add("name", "sword_0");
    param.Add("atk", 999);
    // 아이템 지급
    bro = Backend.GameInfo.Insert("item", param);
    if(bro.IsSuccess() == false)
    {
      return ReturnErrorObject("fail to insert item: " + bro.GetMessage());
    }

    return Backend.StringToStream("success");
}
```

---
####
3. Backend CLI config.json을 설정합니다.  

cmd에서 `backend config`를 입력하고 열린 메모장에서 authKey를 제외하고 아래와 같이 입력 후 저장을 합니다.  

```js
{
  "account": {
    "authKey": "뒤끝 콘솔에서 발급받은 뒤끝펑션 인증키"
  },
  "projectInfo": {
    // 개발자가 생성한 프로젝트의 절대 경로를 입력합니다.  
    // 경로에는 반드시 \\로 구분해야 CLI에서 정상적으로 인식할 수 있습니다.  
    // ex) C:\\work\\bf
    "projectPath": "프로젝트 경로",
    // 개발사에서 생성한 프로젝트의 명을 입력합니다.  
    "csprojName": "BackendFunction.csproj",
    "binName": "publish.zip"
  },
  "functionName": "giveSword",
  "description": "영수증 검증 함수"
}
```

---
####
4. Backend CLI를 이용하여 해당 프로젝트를 빌드합니다.  

cmd에서 `backend build` 명령어를 입력합니다.  
[빌드 문서](/sdk-docs/function/cli/build)를 참고해 주세요.  

---
####
5. Backend CLI를 이용하여 빌드 결과물을 서버로 배포합니다.  

cmd에서 `backend deploy giveSword` 명령어를 입력합니다.  
[배포 문서](/sdk-docs/function/cli/deploy)를 참고해 주세요.  

---
####
6. 뒤끝콘솔에서 정상적으로 배포가 되었는지 확인합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/tutorial/giveSword.png)

---
####
7. [뒤끝펑션에서 영수증 검증하기 문서](/sdk-docs/backend/base/function/receipt-and-tbc/receipt-and-recharge-tbc)를 참고하여 인앱결제 진행 후 배포한 함수가 정상적으로 리턴되는지 확인합니다.  

뒤끝 SDK에서 아래와 같이 함수를 호출해보고, 정상적으로 리턴되는지 확인합니다.  

```js
public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs args)
{
    string id = string.Empty;
    string token = string.Empty;
    Param param = new Param();

#if UNITY_IOS
    // ios의 경우 PurchaseEventArgs에 포함되어 있는 영수증 토큰을 그대로 뒤끝펑션으로 송신하면 됩니다.  
    param.Add("token", args.purchasedProduct.receipt);
#elif UNITY_ANDROID
    // android의 경우 PurchaseEventArGs에 포함되어 있는 영수증 토큰을
    // BackEnd.Game.Payment.GoogleReceiptData.FromJson 함수를 이용하여 파싱하여 id와 token 값을 추출한 뒤
    // 이를 뒤끝펑션으로 송신해야 합니다.  
    BackEnd.Game.Payment.GoogleReceiptData.FromJson(args.purchasedProduct.receipt, out id, out token);
    param.Add("productID", id);
    param.Add("token", token);
#endif

    // 뒤끝펑션 호출
    Backend.BFunc.InvokeFunction("receiptVaildate", param, callback => {
        if(callback.IsSuccess() == false)
        {
          Debug.LogError("뒤끝펑션 실행 실패: "+callback);
          return;
        }
        var result = callback.GetReturnValuetoJSON()["result"].ToString();
            Debug.Log("뒤끝펑션 실행 결과: " + result);
    });

    return PurchaseProcessingResult.Complete;
}
```

**성공하면 callback이 아래와 같이 리턴됩니다.**  
statusCode : 200  
message : Success  
returnValue : {"result":"success"}
