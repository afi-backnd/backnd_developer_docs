---
sidebar_label: 쿠폰 리스트 조회
---

# GetCoupons
public Task&lt;GetCouponsResult&gt; **GetCouponsAsync**();

## 설명
뒤끝 콘솔에 등록한 쿠폰의 리스트를 조회합니다.  
* 신규 쿠폰과 구버전 쿠폰을 모두 조회할 수 있습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndCoupon.Instance.GetCouponsAsync();
```

### Callback 방식
```js
BackndCoupon.Instance.GetCoupons((callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

## ReturnValueJson
```js
{
  rows: [
    {
      // 쿠폰 uuid
      uuid: "296",
      // 쿠폰 명
      title: "쿠폰 명",
      // 쿠폰 타입(시리얼: serial / 단일: single)
      type: "serial",
      // 버전정보(신규쿠폰: new / 구버전 쿠폰: old)
      version: "new"
      // 중복 사용 여부(중복 사용 허용: y, 중복 사용 불가: n)
      redundant: "n"
    },
    {
      uuid: [String],
      title: [String],
      type: [String],
      version: [String],
      redundant: [String]
    },
    ...  
  ];
}
```

## Sample Code
```js
public class Coupon
{
    public string uuid; // 쿠폰 아이디
    public string title; // 쿠폰 명
    public string type;// 시리얼/구버전
    public string version; // 신규버전인지
    public string redundant; // 중복가능한 쿠폰인지
    public override string ToString()
    {
        return $"title: {title}\n" +
        $"uuid: {uuid}\n" +
        $"type: {type}\n" +
        $"version: {version}\n" +
        $"redundant: {redundant}";
    }
}
```

```js
public async Task CouponListTest()
{
    var reqResult = await BackndCoupon.Instance.GetCouponsAsync();
    if (!reqResult.IsSuccess())
    {
        Debug.LogError("에러가 발생했습니다 : " + reqResult.ToString());
        return;
    }

    List<Coupon> couponList = new List<Coupon>();

    var json = reqResult.GetRows();
    for (int i = 0; i < json.Count; i++)
    {
        Coupon coupon = new Coupon();

        coupon.title = json[i]["title"].ToString();
        coupon.uuid = json[i]["uuid"].ToString();
        coupon.type = json[i]["type"].ToString();
        coupon.version = json[i]["version"].ToString();
        coupon.redundant = json[i]["redundant"].ToString();

        couponList.Add(coupon);
    }

    foreach (var coupon in couponList)
    {
        Debug.Log(coupon.ToString() + "\n");
    }
}
```
