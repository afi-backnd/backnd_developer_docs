---
sidebar_position: "2"
description: "웹 쿠폰 설정"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 웹 쿠폰 설정

**해당 기능은 SDK 버전 5.9.2 이상에서 이용 가능합니다.**  

웹 쿠폰을 설정할 수 있는 기능을 제공합니다.  
이용자가 직접 쿠폰코드를 입력할 수 있는 웹페이지 입니다.  

- 상단 및 본문 이미지와 내용 텍스트를 입력할 수 있습니다.(선택 입력)
- 상단 및 본문 이미지는 최대 1MB까지 업로드 가능합니다.  
- 내용 텍스트는 500자 까지 입력 가능합니다.  
- **적용** 버튼을 클릭하면 이미지 및 내용이 웹페이지에 반영됩니다.  

<ConsoleLinkButton text="웹 쿠폰 설정 바로가기" menu="baseCoupon/webCoupon" feature="쿠폰" title="웹 쿠폰 설정" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/웹쿠폰페이지_a1.png" />

이미지 및 내용을 입력 후 **적용** 버튼을 클릭합니다.  
이미지 업로드 이후 **x** 아이콘을 클릭하여 이미지를 초기화할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/웹쿠폰페이지_a2.png" />

웹 쿠폰 페이지의 URL이 생성됩니다.  
아이콘을 눌러 URL을 복사할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/웹쿠폰페이지_a3.png" />

URL로 접속하면 페이지를 확인할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/웹쿠폰페이지_스크린샷.png" />

### UID를 발급하는 방법

뒤끝에서 사용하는 UID는 클라이언트에서 유저가 로그인 이후에 발급받는 UID 값입니다.  
UID를 발급받는 방법에 대해서는 [개발자 문서 - 웹 쿠폰 사용 방법](/sdk-docs/backend/base/coupon/wep-coupon) 문서를 참고해주세요.  

### UID가 입력된 웹 쿠폰 URL을 생성하는 방법

복사한 URL에 UID 파라미터를 추가하여 웹 쿠폰 페이지에 접속했을 때 UID가 입력되어 있게 할 수 있습니다.  

예) `https://storage.thebackend.io/****~/coupon.html?lng=ko` → `https://storage.thebackend.io/****~/coupon.html?lng=ko&UID=<기본으로 입력될 UID>`

**결과값**  

<img src="https://developer.thebackend.io/static/img/unity/coupon/coupon_web.png" />
