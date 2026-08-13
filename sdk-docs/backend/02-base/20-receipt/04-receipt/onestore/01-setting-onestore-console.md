---
sidebar_label: "원스토어 결제 콘솔 설정"
description: "원스토어 결제 콘솔 설정"
---

# 원스토어 결제 콘솔 설정

원스토어 환경에서 뒤끝을 이용하여 원스토어 결제 영수증의 유효성 검증을 위해서는 원스토어 개발자 페이지에서 아래와 같은 설정을 해야 합니다.  

:::info 구독 상품 사용 불가 안내
구독 상품의 영수증 검증 기능은 현재 사용하실 수 없습니다.
:::

### 1. 원스토어 In-App 상품 등록하기

1. [ONE store developer center](https://dev.onestore.co.kr/devpoc/index.omp)원스토어 개발자 페이지에서 **APPS > 상품 현황 > 등록하고자 하는 상품**을 선택합니다.
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/onestore_02.png" />  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/onestore_03.png" />  

2. **상품 현황 > In-App 정보 > 관리 상품 or 구독상품 > In-App 상품** 버튼을 눌러 상품 정보를 입력합니다.  
상품 정보 입력이 완료되었다면 저장을 눌러 상품을 등록합니다.

<img src="https://developer.thebackend.io/static/img/unity/receipt_one/receipt_one_01.png" />
<img src="https://developer.thebackend.io/static/img/unity/receipt_one/receipt_one_02.png" />

### 2. 테스터 등록하기

1. **상품 현황 > In-App 정보 > 결제테스트** 탭으로 진입합니다.
2. **테스트ID**칸에 ID를 입력한 후 조회를 눌러 유저를 조회하고, 정보를 입력 후 등록 버튼을 눌러줍니다.
<img src="https://developer.thebackend.io/static/img/unity/receipt_one/receipt_one_03.png" />

3. 테스터 등록이 성공적으로 이루어졌다면 하단 테스트ID 관리에서 해당 유저의 정보가 표시됩니다.
<img src="https://developer.thebackend.io/static/img/unity/receipt_one/receipt_one_04.png" />

### 3. 뒤끝 콘솔 연결

1. 뒤끝 콘솔에서 원스토어의 인앱에 대한 영수증 검증을 하려면 **뒤끝 콘솔 > 프로젝트 설정 > 스토어 정보**에서 다음 값을 입력해야합니다.

![](/img/docs/guide/base/onestore/backend-value.png)


* Client ID
* Client Secret

해당 정보는 원스토어의 **APPS > In-App 정보 > 관리 상품 창에서 In-App API 관리**를 클릭하여 확인할 수 있습니다.

![](/img/docs/guide/base/onestore/onestore-value.png)
