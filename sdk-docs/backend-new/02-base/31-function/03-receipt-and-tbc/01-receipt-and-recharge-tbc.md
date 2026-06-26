---
sidebar_label: 영수증 검증/TBC 충전
---

# 영수증 검증 / TBC 충전

영수증 검증을 하기 위해서는 먼저 인앱결제를 진행한 후 영수증 토큰을 발급받아야 합니다.  
인앱 결제를 진행하는 방법에 대해서는 아래 링크를 참고해 주세요.  

- [구글 인앱결제](/sdk-docs/backend/base/receipt/android/verify)
- [애플 인앱결제](/sdk-docs/backend/base/receipt/ios/verify)

---

# FromJson

**FromJson**(string **json**, out string **productId**, out string **purchaseToken**) ;

## 파라미터

| Value         | Type   | Description                  |
| :------------ | :----- | :--------------------------- |
| json          | string | 구매 후 발급되는 영수증 토큰 |
| productId     | string | (out) 상품 ID                |
| purchaseToken | string | (out) 실제 영수증 토큰       |

## 설명

구글 영수증을 뒤끝펑션 내에서 영수증 검증을 하기 위해서는 이 함수를 이용하여 영수증의 productId와 purchaseToken 값을 파싱 한 다음 해당 값을 뒤끝펑션으로 송신해야 합니다.  

## Example

```js
string pid = string.Empty;
string token = string.Empty;
BackndIAP.Google.FromJson("구글 영수증 토큰", out pid, out token);
```
