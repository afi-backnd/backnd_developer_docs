---
sidebar_label: 요청 성공/실패 여부 확인
---

# IsSuccess
public bool **IsSuccess**();

## 설명
요청이 성공했는지 실패했는지 확인합니다.  
* statusCode가 200번 대인 경우 성공으로 리턴합니다.  
* statusCode가 300 이상인 경우 실패로 리턴합니다.  

## Example
```js
var reqResult = await BackndCoupon.Instance.UseCouponAsync("쿠폰코드");
if (reqResult.IsSuccess() == false)
{
    // 요청 실패 처리
}

// 요청 성공 처리
```

## Return Cases
**요청이 성공한 경우**  
true

**요청이 실패한 경우**  
false
