---
sidebar_label: 쿠폰 사용하기
description: "쿠폰 사용하기"
---

# UseCoupon
public Task&lt;UseCouponResult&gt; **UseCouponAsync**(string **couponCode**);

## 파라미터

| Value|  Type | Description |
| --- | --- | --- |
| couponCode | string | 뒤끝 콘솔에서 발급된 쿠폰 코드 |

## 설명
콘솔에 등록한 쿠폰을 사용합니다.  
* 신규 쿠폰과 구버전 쿠폰을 모두 사용할 수 있습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndCoupon.Instance.UseCouponAsync("18c7c2e1d16e779b");
```

### Callback 방식
```js
BackndCoupon.Instance.UseCoupon("18c7c2e1d16e779b", (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**아이템이 존재하지 않는 쿠폰을 사용한 경우**  
  statusCode : 200  
  message : Success  
  returnValue : {"uuid":613}

**아이템이 존재하는 신규 쿠폰을 사용한 경우**  
  statusCode : 200  
  message : Success  
  returnValue : ReturnValueJson 신규 쿠폰 참조

**아이템이 존재하는 구버전 쿠폰을 사용한 경우**  
  statusCode : 200  
  message : Success  
  returnValue : ReturnValueJson 구버전 쿠폰 참조

### Error cases

**중복이 허용되지 않는 시리얼 쿠폰을 중복 사용한 경우**  
  statusCode : 404  
  errorCode : NotFoundException  
  message : 이미 사용되었거나, 틀린 번호입니다. not found, 이미 사용되었거나, 틀린 번호입니다.을(를) 찾을 수 없습니다

**단일 쿠폰의 회수율이 100%인 경우**  
  statusCode : 404  
  errorCode : NotFoundException  
  message : 전부 사용된 쿠폰입니다. not found, 전부 사용된 쿠폰입니다.을(를) 찾을 수 없습니다

**기간 만료된 쿠폰의 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : 이미 사용되었거나, 틀린 번호입니다. not found, 이미 사용되었거나, 틀린 번호입니다.을(를) 찾을 수 없습니다

**한 명의 게이머가 단일 쿠폰을 두 번 이상 사용 시도한 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : 이미 사용하신 쿠폰입니다. not found, 이미 사용하신 쿠폰입니다.을(를) 찾을 수 없습니다


## ReturnValueJson
```js
// 신규 쿠폰
{
      // 쿠폰 uuid
      uuid: "400",
      // 신규 쿠폰은 2개 이상의 아이템을 쿠폰에 등록할 수 있기 때문에,
      // 리스트로 아이템이 리턴됩니다.  
      itemObject : [
          {
              item: {
                   // 차트 파일 이름
                   chartFileName: "chartExample.xlsx",
                   // 아이템 정보
                   itemID: "i101",
                   itemName: "아이템1",
                   hpPower: "1",
                   num: "1"
              },
              itemCount: "1" // 지급되는 아이템 개수
          },
          ...  
      ]
}

// 구버전 쿠폰
{
    // 쿠폰 uuid
    uuid: "628",
    // 쿠폰에 등록한 아이템 정보
    items:{ 
        // 차트 파일 이름
        chartFileName: "chartExample.xlsx",
        // 아이템 정보
        itemID: "i101",
        itemName: "아이템1",
        hpPower: "1",
        num: "1"
    },
    itemCount: "1" // 지급되는 아이템 개수
}
```

## Sample Code
```js
// 뒤끝의 기본 제공 차트를 이용하면 만든 아이템입니다.  
// 업로드하신 차트의 컬럼명에 맞게 변수를 변경해주시기 바랍니다.  
public class CouponItem
{
    public string uuid;
    public string chartFileName;
    public string itemID;
    public string itemName;
    public int hpPower;
    public int itemCount;
    public override string ToString()
    {
        return $"itemID : {itemID}\n" +
        $"itemName : {itemName}\n" +
        $"hpPower : {hpPower}\n" +
        $"itemCount : {itemCount}\n" +
        $"chartFileName : {chartFileName}\n" +
        $"uuid : {uuid}\n";
    }
}

public async Task UseCouponTest()
{
    string couponUUID = "2d64e2210493ca6cf5";

    var reqResult = await BackndCoupon.Instance.UseCouponAsync(couponUUID);
    if (!reqResult.IsSuccess())
    {
        Debug.LogError(reqResult.ToString());
        return;
    }

    List<CouponItem> couponItemList = new List<CouponItem>();

    var json = reqResult.GetJObject();
    var rows = reqResult.GetRows("itemObject");
    for (int i = 0; i < rows.Count; i++)
    {
        CouponItem couponItem = new CouponItem();

        couponItem.uuid = json["uuid"].ToString();
        couponItem.itemCount = int.Parse(rows[i]["itemCount"].ToString());
        couponItem.itemID = rows[i]["item"]["itemID"].ToString();
        couponItem.itemName = rows[i]["item"]["itemName"].ToString();
        couponItem.hpPower = int.Parse(rows[i]["item"]["hpPower"].ToString());
        couponItem.chartFileName = rows[i]["item"]["chartFileName"].ToString();

        couponItemList.Add(couponItem);
    }

    foreach (var couponItem in couponItemList)
    {
        Debug.Log(couponItem.ToString());
    }
}
```
