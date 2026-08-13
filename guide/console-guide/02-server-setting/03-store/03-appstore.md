---
description: "앱 스토어"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 앱 스토어

## 앱 스토어 정보 입력

다음 정보는 영수증 검증 기능 이용 및 대시보드의 매출 관련 지표를 제공하기 위해 필요한 정보입니다.  
해당 정보를 입력 시, 뒤끝 콘솔의 영수증 검증 기능 및 대시보드의 매출 관련 지표 정보를 이용할 수 있습니다.  
해당 정보는 모두 [App Store Connect](https://appstoreconnect.apple.com/)에서 획득할 수 있습니다.  

- SKU
- Vendor Number(공급업체 번호)
- Issuer ID
- Key ID
- API Key(P8 파일)

<ConsoleLinkButton text="스토어 정보 바로가기" menu="settingStore" feature="스토어 정보" title="앱 스토어" />

> <img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정스토어-정보---앱-스토어.png" />

## SKU

SKU는 Apple store에 앱을 등록할 때 생성한 앱의 고유한 ID로, 번들 ID와 별개의 값입니다.  
SKU는 유료 앱의 매출정보를 확인하기 위해 필요한 정보입니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore1-1.png" />

  


<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore1-2.png" />

SKU를 **뒤끝 콘솔 → 서버 설정 → 스토어 정보 → 앱 스토어 → SKU**에 입력합니다.  
<img src="https://developer.thebackend.io/static/img/console/store/apple/1-3.png" />

## Vendor Number(공급업체 번호)

공급업체 번호의 #을 제외한 숫자만 **뒤끝 콘솔 → 서버 설정 → 스토어 정보 → 앱 스토어 → Vendor Number**에 입력합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore2-1.png" />

  


<img src="https://developer.thebackend.io/static/img/console/store/apple/2-2.png" />

## App Store Connect API Access 요청

App Store Connect API의 Key를 발급받기 위해서, 먼저 Access를 요청해야 합니다.  
이 과정은 신청 후, 애플의 승인이 필요하며 승인되기까지 약 일주일 이상의 시간이 소요될 수도 있습니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore-3-1.png" />

  


<img src="https://developer.thebackend.io/static/img/console/store/apple/6-1.png" />

애플 승인 후, API Key를 생성합니다.  
<img src="https://developer.thebackend.io/static/img/console/store/apple/6-2.png" />

## Issuer ID

Issuer ID는 인증 토큰을 생성한 발급자 식별에 사용되는 고유한 ID입니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore-3-1.png" />

Issuer ID를 **뒤끝 콘솔 → 서버 설정 → 스토어 정보 → 앱 스토어 → Issuer ID**에 입력합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore-3-2.png" />

## Key ID

App Store Connect API에 접근하기 위한 키의 ID입니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore-3-3.png" />

키 생성 시, 재정관리 권한을 필수로 넣어줍니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore-3-4.png" />

생성된 키 ID를 **뒤끝 콘솔 → 서버 설정 → 스토어 정보 → 앱 스토어 → KEY ID**에 입력합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore-3-5.png" />

## API Key(P8 파일)

App Store Connect API에 접근하기 위한 키 파일입니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore-3-6.png" />

API 키를 다운로드해 **뒤끝 콘솔 → 서버 설정 → 스토어 정보 → 앱 스토어 → API KEY**에 업로드합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/edit/appstore-3-7.png" />


## Apple In-app PurchaseKey

앱 스토어의 입앱결제 데이터를 조회하기 위한 값입니다.
- 경로: Apple Developer > App Store Connect > Users and Access > Keys > In-App Purchases

![image](/img/docs/guide/base-serversetting/store_apple1.png)
![image](/img/docs/guide/base-serversetting/store_apple2.png)
