---
sidebar_label: "[Legacy] 약관 및 정책"
sidebar_position: "7"
description: "GetPolicyV2"
---

# GetPolicyV2

public BackendReturnObject **GetPolicyV2**();

:::danger GetPolicy 함수 GetPolicyV2 마이그레이션 안내
2023.03.28 콘솔 업데이트로 인해 게임 운영 정책이 2개까지 생성 가능하도록 변경되었습니다.  

값의 구분을 위해 리턴값이 일부 변경되었으며, GetPolicy에서 GetPolicyV2로 마이그레이션 할 경우 다음과 같이 코드를 변경하면 됩니다.  

- 변경 전 : Backend.Policy.GetPolicy().GetReturnValuetoJSON()["terms"].ToString();  

- 변경 후 : Backend.Policy.GetPolicyV2().GetReturnValuetoJSON()["policy"]["terms"].ToString();
:::


## 설명
뒤끝 콘솔에 등록한 서비스 이용 약관과 개인 정보 처리 방침들을 불러오는 기능입니다.  
* 프로젝트 생성 이후 서비스 이용 약관 혹은 개인 정보 처리 방침을 수정하지 않았을 경우 해당 값들은 null로 리턴됩니다.  
* **추가** 정책의 서비스 이용 약관 혹은 개인 정보 처리 방침 모두 수정되지 않은 상태라면 **GetPolicyV2**의 리턴값에서는 노출되지 않습니다.  

## Example

### 동기
```js
BackendReturnObject bro = Backend.Policy.GetPolicyV2();

string KoreanPrivacyUrl = bro.GetReturnValuetoJSON()["policy"]["privacyURL"].ToString();

// 추가 정책을 한번이라도 수정한 경우, policy2가 노출됩니다.  
if(bro.GetReturnValuetoJSON().ContainsKey("policy2") {
    string EnglishTermsUrl = bro.GetReturnValuetoJSON()["policy2"]["termsURL"].ToString();
}

```

### 비동기
```js
Backend.Policy.GetPolicyV2((callback) => {
    string KoreanPrivacyUrl = bro.GetReturnValuetoJSON()["policy"]["privacyURL"].ToString();

    // 추가 정책을 한번이라도 수정한 경우
    if(bro.GetReturnValuetoJSON().ContainsKey("policy2") {
        string EnglishTermsUrl = bro.GetReturnValuetoJSON()["policy2"]["termsURL"].ToString();
    }
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.Policy.GetPolicyV2, (callback) => {
    string KoreanPrivacyUrl = bro.GetReturnValuetoJSON()["policy"]["privacyURL"].ToString();

    // 추가 정책을 한번이라도 수정한 경우
    if(bro.GetReturnValuetoJSON().ContainsKey("policy2") {
        string EnglishTermsUrl = bro.GetReturnValuetoJSON()["policy2"]["termsURL"].ToString();
    }
});
```

## ReturnCase

### Success cases
**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**기본 정책이 등록되어 있지 않는 경우**  
statusCode : 200  
message : Success  
returnValue : {"policy":{"terms":null,"termsURL":null,"privacy":null,"privacyURL":null}}

## GetReturnValuetoJSON
**기본 정책만 수정한 경우**  
```js
{
  "policy": {
    "terms": "서비스 이용약관",
    "termsURL": "storage.alpha.thebackend.io/89abe12345657889901231232876f51145c54724af44d58396694a6fd/terms.html",
    "privacy": "개인정보처리방침",
    "privacyURL": "storage.alpha.thebackend.io/89abe12345657889901231232876f51145c54724af44d58396694a6fd/privacy.html"
  }
}
```

**기본 정책과 추가 정책 모두 수정한 경우**  
```js
{
  "policy": {
    "terms": "서비스 이용약관",
    "termsURL": "storage.alpha.thebackend.io/89abe12345657889901231232876f51145c54724af44d58396694a6fd/terms.html",
    "privacy": "개인정보처리방침",
    "privacyURL": "storage.alpha.thebackend.io/89abe12345657889901231232876f51145c54724af44d58396694a6fd/privacy.html"
  },
  "policy2": {
    "terms": "Terms of Service",
    "termsURL": "storage.thebackend.io/89abe12345657889901231232876f51145c54724af44d58396694a6fd/terms2.html",
    "privacy": "Privacy Policy",
    "privacyURL": "storage.thebackend.io/89abe12345657889901231232876f51145c54724af44d58396694a6fd/privacy2.html"
  }
}
```

## Sample Code
```js
public class Policy
{
    public string terms;
    public string termsURL;
    public string privacy;
    public string privacyURL;
    public override string ToString()
    {
        string str = $"terms : {terms}\n" +
        $"termsURL : {termsURL}\n" +
        $"privacy : {privacy}\n" +
        $"privacyURL : {privacyURL}\n";
        return str;
    }
}
public void GetPolicyV2Test() {
	var bro = Backend.Policy.GetPolicyV2();

	if(!bro.IsSuccess()) {
		return;
	}

	PolicyItem policy = new PolicyItem();

	LitJson.JsonData policyJson = bro.GetReturnValuetoJSON()["policy"]; // policy로 접근

	policy.terms = policyJson["terms"]?.ToString(); // 입력이 없을 경우 null
	policy.termsURL = policyJson["termsURL"]?.ToString(); // 입력이 없을 경우 null
	policy.privacy = policyJson["privacy"]?.ToString(); // 입력이 없을 경우 null
	policy.privacyURL = policyJson["privacyURL"]?.ToString(); // 입력이 없을 경우 null
	
	Debug.Log(policy.ToString());

	if(bro.GetReturnValuetoJSON().ContainsKey("policy2")) {

		LitJson.JsonData policy2Json = bro.GetReturnValuetoJSON()["policy2"]; // policy2로 접근

		PolicyItem policy2 = new PolicyItem();

		policy2.terms = policy2Json["terms"]?.ToString(); // 입력이 없을 경우 null
		policy2.termsURL = policy2Json["termsURL"]?.ToString(); // 입력이 없을 경우 null
		policy2.privacy = policy2Json["privacy"]?.ToString(); // 입력이 없을 경우 null
		policy2.privacyURL = policy2Json["privacyURL"]?.ToString(); // 입력이 없을 경우 null

		Debug.Log(policy2.ToString());
	}
}
```
