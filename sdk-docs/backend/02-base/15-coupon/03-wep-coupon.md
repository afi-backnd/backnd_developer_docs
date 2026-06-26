# 웹 쿠폰 사용 방법

## 유저 사용 웹페이지 생성 방법
유저가 발급받은 쿠폰 코드를 사용하는 웹 페이지를 생성하는 방법은 [콘솔 가이드 - 웹 쿠폰 설정](/guide/console-guide/backnd-base/coupon/web-coupon) 페이지를 참고해주세요.  

## UID 발급 방법
웹 쿠폰 페이지에서 사용하는 UID는 클라이언트에서 유저가 로그인 이후에 발급받는 UID 값입니다.  
다음과 같은 로직으로 확인이 가능합니다.  

<img src="https://developer.thebackend.io/static/img/unity/coupon/web-coupon-default.png" />

### UID 발급 로직
```js
// 뒤끝 로그인
var bro = Backend.BMember.CustomLogin("backendUser", "backendUser");

// 로그인/회원가입이 성공할 경우에만 UID값이 발급됩니다.  
if(bro.IsSuccess()) {
    Debug.Log("로그인에 성공했습니다 : " + bro);
    Debug.Log("유저 UID(쿠폰용) : " + Backend.UID);
}
else {
    Debug.LogError("로그인 중 에러가 발생했습니다.");
    // Backend.UserNickname, Backend.UserInDate, Backend.UID에 값이 할당되지 않습니다.  
}
```

<img src="https://developer.thebackend.io/static/img/unity/coupon/success-log.png" />

## 웹 쿠폰 사용 이후
유저의 UID와 제공된 쿠폰코드를 사용하여 웹 쿠폰 페이지에서 쿠폰을 사용합니다.  
쿠폰 사용을 성공하였다면 해당 쿠폰에 등록된 아이템은 쿠폰 우편에서 확인할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/unity/coupon/coupon-use-success.png" />

## 웹 쿠폰 함수 이용 방법
웹 쿠폰 페이지에서 쿠폰을 사용하는 방법 외에도 SDK 함수로 웹 쿠폰을 사용할 수 있습니다.  

### 동기
```js
string webCouponUrl = "https://storage.thebackend.io/1ea3f14d34e89....이하생략...e821ce49c7845/coupon.html?lng=ko";
string couponCode = "WELCOME"

Backend.Coupon.UseWebCoupon(webCouponUrl, Backend.UID, couponCode);
```

### 비동기
```js
string webCouponUrl = "https://storage.thebackend.io/1ea3f14d34e89....이하생략...e821ce49c7845/coupon.html?lng=ko";
string couponCode = "WELCOME"

Backend.Coupon.UseWebCoupon(webCouponUrl, Backend.UID, couponCode, (callback) => 
{
    // 이후 처리
});
```

## 쿠폰 우편 리스트 불러오기
쿠폰 사용이 성공하였다면 해당 쿠폰은 우편으로 발송됩니다.  
쿠폰 우편 사용 방법은 관리자 우편 및 랭킹 우편의 불러오기, 수령하기, 모두 수령하기 기능과 동일합니다.  
쿠폰 우편에 대해서는 [우편 기능 - 쿠폰 우편 불러오기](/sdk-docs/backend/base/post/get-list/coupon) 문서를 참고해주세요.  

## 웹 쿠폰 url 이용시 uid 자동 입력 기능 추가
웹 쿠폰 url에 uid 파라미터 `&uid={UID번호}`를 추가하여  페이지 이동 시 자동으로 uid를 등록할 수 있습니다.  
> uid가 반영되지 않을 경우, 웹 쿠폰 관리에서 **적용**버튼을 클릭해주시기 바랍니다.  

**예제코드**  
```js
string webCouponUrl = "https://storage.thebackend.io/1ea3f14d34e89....이하생략...e821ce49c7845/coupon.html?lng=ko";
string uidParameterUrl = $"&uid={Backend.UID}";
// example : "&uid=736274625"
Application.OpenURL(webCouponUrl + uidParameterUrl);
```

**결과값**  
<img src="https://developer.thebackend.io/static/img/unity/coupon/webcoupon_uid.png" />
