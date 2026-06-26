# GetPolicy

public Task&lt;GetPolicyResult&gt; **GetPolicyAsync**();

## 설명
뒤끝 콘솔에 등록한 서비스 이용 약관과 개인 정보 처리 방침들을 불러오는 기능입니다.  
* 프로젝트 생성 이후 서비스 이용 약관 혹은 개인 정보 처리 방침을 수정하지 않았을 경우 해당 값들은 null로 리턴됩니다.  
* **추가** 정책의 서비스 이용 약관 혹은 개인 정보 처리 방침 모두 수정되지 않은 상태라면 **GetPolicy**의 리턴값에서는 노출되지 않습니다.  

## Example

### Task 형식
```js
var reqResult = await BackndPolicy.Instance.GetPolicyAsync();
if (reqResult.IsSuccess())
{
    var korPrivacyUrl = reqResult.GetInfo("policy").PrivacyUrl;
    var englishTermsUrl = reqResult.GetInfo("policy2").TermsUrl;
}
```

### Callback 형식
```js
BackndPolicy.Instance.GetPolicy((callback) =>
{
    var korPrivacyUrl = reqResult.GetInfo("policy").PrivacyUrl;

    // 추가 정책을 한번이라도 수정한 경우
    var policy2Info = reqResult.GetInfo("policy2");
    if (policy2Info != null)
    {
        var englishTermsUrl = policy2Info.TermsUrl;
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
public class PolicyItem
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

public async Task GetPolicy()
{
    var reqResult = await BackndPolicy.Instance.GetPolicyAsync();
    if (!reqResult.IsSuccess())
    {
        return;
    }

    var policyInfo = reqResult.GetInfo("policy"); // policy로 접근        
    var policy = new PolicyItem();
    policy.terms = policyInfo.Terms; // 입력이 없을 경우 null
    policy.termsURL = policyInfo.TermsUrl; // 입력이 없을 경우 null
    policy.privacy = policyInfo.Privacy; // 입력이 없을 경우 null
    policy.privacyURL = policyInfo.PrivacyUrl; // 입력이 없을 경우 null

    Debug.Log(policy.ToString());

    var policy2Info = reqResult.GetInfo("policy2"); // policy2로 접근    
    if (policy2Info != null)
    {
        var policy2 = new PolicyItem();
        policy2.terms = policy2Info.Terms; // 입력이 없을 경우 null
        policy2.termsURL = policy2Info.TermsUrl; // 입력이 없을 경우 null
        policy2.privacy = policy2Info.Privacy; // 입력이 없을 경우 null
        policy2.privacyURL = policy2Info.PrivacyUrl; // 입력이 없을 경우 null

        Debug.Log(policy2.ToString());
    }
}
```
