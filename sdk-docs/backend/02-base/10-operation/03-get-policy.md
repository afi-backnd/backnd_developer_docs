---
sidebar_label: "약관 및 정책"
sidebar_position: "3"
description: "GetPolicyV3"
---

# GetPolicyV3

public BackendReturnObject **GetPolicyV3**();

## 설명
뒤끝 콘솔에 등록한 서비스 이용 약관과 개인 정보 처리 방침들을 불러오는 기능입니다.
* 콘솔에서 설정한 언어명이 키 값으로 사용됩니다.
* 해당 언어의 서비스 이용 약관과 개인 정보 처리 방침이 모두 등록되지 않은 경우 해당 언어는 리턴값에 포함되지 않습니다.

## Example

### 동기
```csharp
const string LANG_KOREAN = "한국어";
const string LANG_JAPANESE = "jp";

BackendReturnObject bro = Backend.Policy.GetPolicyV3();

// 콘솔에서 설정한 언어명으로 접근
string koreanPrivacyUrl = bro.GetReturnValuetoJSON()[LANG_KOREAN]["privacy"]["url"].ToString();
string koreanTermsContent = bro.GetReturnValuetoJSON()[LANG_KOREAN]["terms"]["content"].ToString();

// 다른 언어가 등록된 경우
if(bro.GetReturnValuetoJSON().ContainsKey(LANG_JAPANESE)) {
    string jpTermsUrl = bro.GetReturnValuetoJSON()[LANG_JAPANESE]["terms"]["url"].ToString();
}
```

### 비동기
```csharp
const string LANG_KOREAN = "한국어";
const string LANG_JAPANESE = "jp";

Backend.Policy.GetPolicyV3((callback) => {
    // 콘솔에서 설정한 언어명으로 접근
    string koreanPrivacyUrl = callback.GetReturnValuetoJSON()[LANG_KOREAN]["privacy"]["url"].ToString();

    // 다른 언어가 등록된 경우
    if(callback.GetReturnValuetoJSON().ContainsKey(LANG_JAPANESE)) {
        string jpTermsUrl = callback.GetReturnValuetoJSON()[LANG_JAPANESE]["terms"]["url"].ToString();
    }
});
```

### SendQueue
```csharp
const string LANG_KOREAN = "한국어";
const string LANG_JAPANESE = "jp";

SendQueue.Enqueue(Backend.Policy.GetPolicyV3, (callback) => {
    // 콘솔에서 설정한 언어명으로 접근
    string koreanPrivacyUrl = callback.GetReturnValuetoJSON()[LANG_KOREAN]["privacy"]["url"].ToString();

    // 다른 언어가 등록된 경우
    if(callback.GetReturnValuetoJSON().ContainsKey(LANG_JAPANESE)) {
        string jpTermsUrl = callback.GetReturnValuetoJSON()[LANG_JAPANESE]["terms"]["url"].ToString();
    }
});
```

## ReturnCase

### Success cases
**불러오기에 성공한 경우**
statusCode : 200
message : Success
returnValue : GetReturnValuetoJSON 참조

**정책이 등록되어 있지 않은 경우**
statusCode : 200
message : Success
returnValue : {}

## GetReturnValuetoJSON
**하나의 언어만 등록한 경우**
```json
{
  "한국어": {
    "terms": {
      "title": "한국어",
      "content": "한국어 이용약관",
      "url": "storage.alpha.thebackend.io/f6f485cc044f6c679ea0aed45d6c9a3c427ce4682db926a1a41a7ffa85bd9249/terms한국어.html"
    },
    "privacy": {
      "title": "한국어",
      "content": "한국어 개인정보 처리방침",
      "url": "storage.alpha.thebackend.io/f6f485cc044f6c679ea0aed45d6c9a3c427ce4682db926a1a41a7ffa85bd9249/privacy한국어.html"
    }
  }
}
```

**여러 언어를 등록한 경우**
```json
{
  "한국어": {
    "terms": {
      "title": "한국어",
      "content": "한국어 이용약관",
      "url": "storage.alpha.thebackend.io/f6f485cc044f6c679ea0aed45d6c9a3c427ce4682db926a1a41a7ffa85bd9249/terms한국어.html"
    },
    "privacy": {
      "title": "한국어",
      "content": "한국어 개인정보 처리방침",
      "url": "storage.alpha.thebackend.io/f6f485cc044f6c679ea0aed45d6c9a3c427ce4682db926a1a41a7ffa85bd9249/privacy한국어.html"
    }
  },
  "jp": {
    "terms": {
      "title": "일본어",
      "content": "일본어 이용약관",
      "url": "storage.alpha.thebackend.io/f6f485cc044f6c679ea0aed45d6c9a3c427ce4682db926a1a41a7ffa85bd9249/termsjp.html"
    },
    "privacy": {
      "title": "일본어",
      "content": "일본어 개인정보 처리방침",
      "url": "storage.alpha.thebackend.io/f6f485cc044f6c679ea0aed45d6c9a3c427ce4682db926a1a41a7ffa85bd9249/privacyjp.html"
    }
  }
}
```

## Sample Code
```csharp
// 콘솔에서 설정한 언어명 상수
const string LANG_KOREAN = "한국어";
const string LANG_JAPANESE = "jp";
const string LANG_ENGLISH = "en";

// Unity 기기 언어 → 콘솔 언어명 매핑
private static readonly Dictionary<SystemLanguage, string> LanguageMap = new Dictionary<SystemLanguage, string>
{
    { SystemLanguage.Korean, LANG_KOREAN },
    { SystemLanguage.Japanese, LANG_JAPANESE },
    { SystemLanguage.English, LANG_ENGLISH },
    // 필요한 언어 추가
};

// Fallback 우선순위 (기기 언어 없을 시 순차 시도)
private static readonly string[] FallbackOrder = { LANG_ENGLISH, LANG_KOREAN };

// 정책 데이터에서 사용 가능한 언어 키 반환 (fallback 포함)
public string GetPolicyLanguageKey(Dictionary<string, BackendPolicyItemV3> policyItems)
{
    if(policyItems == null || policyItems.Count == 0) {
        return null;
    }

    SystemLanguage deviceLanguage = Application.systemLanguage;

    // 1순위: 기기 언어와 매핑된 언어
    if(LanguageMap.TryGetValue(deviceLanguage, out string langKey) && policyItems.ContainsKey(langKey)) {
        return langKey;
    }

    // 2순위: Fallback 우선순위에 따라 시도
    foreach(string fallbackLang in FallbackOrder) {
        if(policyItems.ContainsKey(fallbackLang)) {
            return fallbackLang;
        }
    }

    // 3순위: 등록된 첫 번째 언어
    foreach(string key in policyItems.Keys) {
        return key;
    }

    return null;
}

public void GetPolicyV3Test() {
    Backend.Policy.GetPolicyV3(callback =>
    {
        if(!callback.IsSuccess()) {
            Debug.LogError("정책 조회 실패");
            return;
        }

        var policyResult = (BackendPolicyReturnObjectV3)callback;
        var policyItems = policyResult.GetReturnValueByPolicyItems();

        if(policyItems == null || policyItems.Count == 0) {
            Debug.Log("등록된 정책이 없습니다.");
            return;
        }

        // 기기 언어 기반으로 사용 가능한 정책 언어 키 가져오기 (fallback 포함)
        string langKey = GetPolicyLanguageKey(policyItems);

        if(langKey == null) {
            Debug.Log("사용 가능한 정책이 없습니다.");
            return;
        }

        BackendPolicyItemV3 policy = policyItems[langKey];

        Debug.Log($"=== {langKey} ===");
        Debug.Log($"Terms - Title: {policy.Terms.Title}");
        Debug.Log($"Terms - Content: {policy.Terms.Content}");
        Debug.Log($"Terms - Url: {policy.Terms.Url}");
        Debug.Log($"Privacy - Title: {policy.Privacy.Title}");
        Debug.Log($"Privacy - Content: {policy.Privacy.Content}");
        Debug.Log($"Privacy - Url: {policy.Privacy.Url}");
    });
}
```
