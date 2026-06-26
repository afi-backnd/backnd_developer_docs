---
sidebar_label: "[Deprecated] 약관 및 정책"
sidebar_position: 8

draft: true
unlisted: true
---

# [deprecated] GetPolicy

public BackendReturnObject **GetPolicy**();

:::danger GetPolicy 함수 GetPolicyV2 마이그레이션 안내
2023.03.28 업데이트로 인해 게임 운영 정책이 2개까지 생성 가능하도록 변경되었습니다.  

값의 구분을 위해 리턴값이 일부 변경되었으며, GetPolicyV2 로 마이그레이션 할 경우 다음과 같이 코드를 변경하면 됩니다.  

- 변경 전 : Backend.Policy.GetPolicy().GetReturnValuetoJSON()["terms"].ToString();  

- 변경 후 : Backend.Policy.GetPolicyV2().GetReturnValuetoJSON()["policy"]["terms"].ToString();
:::

## 설명

뒤끝 콘솔에 등록한 서비스 이용 약관과 개인 정보 처리 방침을 불러오는 기능입니다.  

## Example

### 동기

```js
BackendReturnObject bro = Backend.Policy.GetPolicy();

string privacyUrl = bro.GetReturnValuetoJSON()["privacyURL"].ToString();
```

### 비동기

```js
Backend.Policy.GetPolicy((callback) => {
    string privacyUrl = callback.GetReturnValuetoJSON()["privacyURL"].ToString();
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Policy.GetPolicy, (callback) => {
    string privacyUrl = callback.GetReturnValuetoJSON()["privacyURL"].ToString();
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**등록되어 있지 않는 경우**  
statusCode : 200  
message : Success  
returnValue : {"terms":null,"termsURL":null,"privacy":null,"privacyURL":null}

## GetReturnValuetoJSON

```js
{
    terms:"서비스 이용 약관입니다", // 아래 서비스 이용 약관 html 파일을 문자열로 파싱하여 리턴합니다.  
    termsURL:"www.thebackend.io/terms.html",
    privacy:"개인 정보 처리 방침입니다.", // 아래 개인 정보 처리 방침 html 파일을 문자열로 파싱하여 리턴합니다.  
    privacyURL:"www.thebackend.io/privacy.html"
}
```

## Sample Code

```js
public class Policy {
    public string terms;
    public string termsURL;
    public string privacy;
    public string privacyURL;
    public override string ToString() {
        string str = $"terms : {terms}\n" +
        $"termsURL : {termsURL}\n" +
        $"privacy : {privacy}\n" +
        $"privacyURL : {privacyURL}\n";
        return str;
    }
}
[TestMethod]
public void GetPolicy() {
    var bro = Backend.Policy.GetPolicy();
    if(!bro.IsSuccess()) {
        return;
    }
    Policy policy = new Policy();
    policy.terms = bro.GetReturnValuetoJSON()["terms"].ToString();
    policy.termsURL = bro.GetReturnValuetoJSON()["termsURL"].ToString();
    policy.privacy = bro.GetReturnValuetoJSON()["privacy"].ToString();
    policy.privacyURL = bro.GetReturnValuetoJSON()["privacyURL"].ToString();
    Debug.Log(policy.ToString());
}
```
