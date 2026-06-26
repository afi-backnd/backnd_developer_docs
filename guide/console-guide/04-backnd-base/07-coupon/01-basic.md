---
sidebar_label: 쿠폰 등록 및 관리
sidebar_position: 1
---
import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 쿠폰 관리

관리자는 콘솔에서 쿠폰 코드를 생성하여 유저에게 제공할 수 있습니다.  
클라이언트의 기능은 [쿠폰 관리](/sdk-docs/backend/base/coupon/get-list)를 통해서 확인해 주세요.  

<ConsoleLinkButton text="쿠폰 바로가기" menu="baseCoupon/coupon" feature="쿠폰" title="쿠폰 관리" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/쿠폰관리_a0.png" />

## 쿠폰 등록

페이지 상단의 명령 버튼 중 **쿠폰 등록** 버튼을 클릭하여 새로운 쿠폰을 생성할 수 있습니다.  
쿠폰은 **쿠폰 분류** 항목의 선택에 따라 시리얼 쿠폰과 단일 쿠폰 중 하나로 생성할 수 있습니다.  

- 쿠폰 명은 최대 20자입니다.  
- 유효 기간은 30분 단위로 설정이 가능합니다.  
- 유효 기간의 디폴트 시작일은 현재의 시간에서 가장 가까운 30분대 시간입니다.(예: 현재 시간이 14:08이면 디폴트는 14:30으로 설정)
- 쿠폰 1회에 시리얼 쿠폰은 최대 50,000개, 단일 쿠폰은 최대 300,000만개까지 발행 가능합니다. 

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/쿠폰관리_a1.png" />

  


<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/쿠폰관리_a2.png" />

### 시리얼 쿠폰

**쿠폰 분류**를 **시리얼**로 선택할 경우, **중복 사용 여부** 항목을 선택할 수 있습니다.  
**중복 사용 여부**를 **허용**할 경우 한 계정이 동일한 쿠폰 내 여러 개의 시리얼 쿠폰을 사용할 수 있으며, **비허용**일 경우 동일한 쿠폰 내 시리얼 쿠폰을 한 계정 당 한 번만 사용할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/쿠폰관리_a3.png" />

### 단일 쿠폰

**쿠폰 분류**를 **단일**로 선택할 경우, **쿠폰 코드** 항목을 입력할 수 있습니다.  
단일 쿠폰의 경우, **쿠폰 코드**를 직접 입력하여 생성할 수 있으며 **자동 생성** 옵션을 체크할 경우 임의의 쿠폰 코드가 생성됩니다.  

- 단일 쿠폰 코드는 2-16자의 영문, 한글, 숫자만 허용되며 영문 대소문자를 구분하지 않습니다.  
- 또한 입력된 쿠폰 코드가 이미 다른 쿠폰에 존재할 경우 생성할 수 없습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/쿠폰관리_a4.png" />

## 쿠폰 조회

해당 쿠폰 명을 클릭하여 쿠폰의 코드나 사용 여부를 조회할 수 있습니다.  
또한 **CSV 다운로드**버튼을 클릭하여 다운로드할 수 있습니다

### 시리얼 쿠폰

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/쿠폰관리_시리얼1_230118.png" />

### 단일 쿠폰

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/쿠폰관리_단일1_230118.png" />

## 쿠폰 삭제

삭제할 쿠폰를 선택한 후 페이지 상단의 명령 버튼 중 **삭제** 버튼을 눌러 쿠폰을 삭제할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/쿠폰 관리/쿠폰관리_a7.png" />
