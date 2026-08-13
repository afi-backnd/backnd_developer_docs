---
description: "구글 플레이 스토어"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 구글 플레이 스토어

:::danger **Google 개발자 계정 간 게임 이관 시**
일반적인 경우 문제가 되지 않지만, **영수증 검증 기능을 이용하는 경우** 변경된 개발자 계정 정보로 스토어 정보 변경이 이루어지기 전까지 **인앱 결제가 실패**하는 현상이 발생합니다.

**게임 이관 전 반드시 안내드리는 방법에 따라 진행해 주세요.**
1. 영수증 검증 기능을 일시적으로 비활성화하여 업데이트
2. 개발자 계정 간 게임 이관
3. 이관된 개발자 계정의 정보로 뒤끝 콘솔 내 스토어 정보 업데이트
4. 영수증 검증 기능 활성화하여 업데이트
:::

## 구글 플레이 정보 입력

다음 정보는 영수증 검증 기능 이용 및 대시보드의 매출 관련 지표를 제공하기 위해 필요한 정보입니다.    
해당 정보를 입력 시, 뒤끝 콘솔의 영수증 검증 기능 및 대시보드의 매출 관련 지표 정보를 이용할 수 있습니다.    
해당 정보는 [구글 플레이 콘솔](https://play.google.com/apps/publish/)에서 획득할 수 있습니다.    

아래 절차를 통해 다음 파일 및 정보를 얻어야 합니다.  
- JWT(Json Web Token) JSON 파일
- 보고서 저장 버킷 URL

:::danger '버킷 URL이 올바르지 않습니다.  ' 메시지가 표시되는 경우 확인해야 할 사항
* 뒤끝 콘솔에서 서버 설정 > 인증정보 > Android Package Name이 올바르게 입력되었는지 확인하세요.  
* JWT 토큰의 권한 설정을 확인하세요.  
* JWT 생성 후, 하루가 지나야 정상적으로 등록이 됩니다.    
* 버킷 URL을 올바르게 설정했는지 확인하세요.  
:::

:::caution 버킷 URL이 존재하지 않는 경우
* <a href="#url2">버킷 URL</a>이 존재하지 않는 경우, 실 결제를 통해 결제 보고서를 생성한 후 url을 확인할 수 있습니다.    
:::
  

## JWT(Json Web Token) JSON 파일 얻기 

JWT를 생성하기 위해서는 다음과 같은 절차가 필요합니다.  

1. 구글 클라우드 플랫폼 접속
2. Google Cloud Platform에 Google Play 접속용 프로젝트 생성
3. 프로젝트에 Google Play 접근 권한 API 설정
4. 해당 프로젝트에 서비스 계정 생성
5. 서비스 계정에서 접근용 키(json) 생성

### 1. 구글 클라우드 플랫폼(Google cloud Platform) 접속
[구글 클라우드 플랫폼](https://console.cloud.google.com/)에 접속합니다.  

### 2. Google Play 접속용 프로젝트 생성
왼쪽 상단에 프로젝트 이름을 클릭하여 프로젝트 리스트를 불러옵니다.    
리스트 창 오른쪽 상단에 **새 프로젝트**를 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/new1.png" />

프로젝트명을 설정합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/new2.png" />

### 3. 프로젝트에 Google Play 접근 권한 API 설정

프로젝트 생성이 완료되었으면 왼쪽 사이드바 탐색 메뉴를 통해 API 및 서비스 > 사용 설정된 API 및 서비스를 클릭합니다.  

혹은 모든 제품에서 API 및 서비스를 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/new3.png" />

상단에 **+API 및 서비스 사용 설정**을 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/new4.png" />

API는 총 2개가 추가되어야 합니다.  
* Google Play Android Developer API
* Google Play Game Services Publishing API

검색창에 Google Play를 검색하고, Google Play Android Developer API를 클릭하여 **사용**으로 변경합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/new5.png" />

Google Play Game Services Publishing API 또한 **사용**으로 변경합니다.  

> <img src="https://developer.thebackend.io/static/img/console/store/google/new7.png" />

해당 버튼을 클릭하면 됩니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/new6.png" />



추가가 모두 완료되면 **Google Play Android Developer API**와 **Google Play Game Services Publishing API**가 리스트에 보여야 합니다.  

두 API가 모두 추가되었다면 Google Play 접근용 프로젝트 생성은 완료되었습니다.  

> <img src="https://developer.thebackend.io/static/img/console/store/google/new8.png" />

### 4. 해당 프로젝트에 서비스 계정 생성

사이드 메뉴에서 **사용자 인증 정보**로 이동합니다.  

* 구글 클라우드 플랫폼에 접속한 후, **+서비스 계정 만들기**를 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-4.PNG" />

* 서비스 계정 이름과 ID 설명을 입력하고 만들기를 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-5.PNG" />

* 역할을 소유자로 선택하고 계속을 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-6.PNG" />

* 서비스 계정 사용자 역할을 현재 구글 콘솔에 접속한 이메일을 입력합니다.   이후 완료를 클릭합니다.  
(입력하지 않을 경우, 구글 콘솔 -API 액세스 내에서 해당 서비스 계정이 보이지 않을 수 있습니다. )
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-7.PNG" />

* 해당 서비스 계정의 권한은 서비스 계정 클릭 후, 권한 탭을 눌러 확인할 수 있습니다.   소유자로 두 계정이 등록되어 있는지 확인해 주시기 바랍니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-7-2.PNG" />

### 5. JWT 생성
만들어진 서비스 계정에서 Json으로 이루어진 키를 만듭니다.  

* 만들어진 서비스 계정을 클릭하고, 세부정보 > 키 > 키 추가를 눌러 **새 키 만들기**를 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-8.PNG" />

키 유형을 Json을 선택하고 **만들기**를 클릭합니다.   이후 자동으로 json이 다운로드 됩니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-9.PNG" />

:::caution JWT 안내 
JWT는 생성이 된지 24시간 ~ 36시간 정도가 지나야 정상적으로 등록이 됩니다.
:::

### 4. 뒤끝 콘솔에 적용
다운로드한 JWT 파일을 뒤끝 콘솔 > 스토어 정보에 등록합니다.  

<ConsoleLinkButton text="스토어 정보 바로가기" menu="settingStore" feature="스토어 정보" title="구글 플레이 스토어" />

><img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정스토어-정보---구글-플레이.png" />

## 서비스 계정 액세스 권한 부여
구글 플레이 콘솔 사이드 메뉴에서 **사용자 및 권한**을 클릭합니다.  

우측 상단에 **신규 사용자 초대**를 클릭합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/new9.png" />

이메일 주소에 생성했던 서비스 계정을 입력합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/new10.png" />

<a id="token"></a>

* 계정 권한에서 **앱 정보 보기 및 보고서 일괄 다운로드(읽기 전용)**, **재무 데이터, 주문, 취소 설문조사 응답 보기** 항목을 체크합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-11.PNG" />

* 앱 권한에서 콘솔에 등록한 애플리케이션을 추가하고, 앱 권한 설정에서 **주문 및 구독 관리** 항목을 체크합니다.  
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-12.PNG" />
> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-13.PNG" />

<br />

## 보고서 저장 버킷 URL
구글 콘솔 > 보고서 다운로드 > 재무 항목에 들어가 **수익 보고서** 또는 **예상 판매 실적 보고서** 우측에 위치한 **Cloud Storage URI 복사**를 클릭하여 보고서 저장 버킷 URL을 복사합니다.  

> <img src="https://developer.thebackend.io/static/img/console/store/google/GPGS-14.PNG" />

<a id="url"></a>
<div id="url2"></div>

`pubsite_prod_xxxxxxxxxxxx` 또는 `pubsite_prod_rev_xxxxxxxxxxxx` 부분을 복사하여 **뒤끝 콘솔 > 서버 설정 > 스토어 정보 > 구글 플레이 > 보고서 저장 버킷 URL**에 등록합니다.  
* 이때 `gs://`는 **제외하고** 복사해야 합니다.  
* 버킷 URL이 존재하지 않는 경우, 실 결제를 통해 결제보고서를 생성한 후 url을 확인할 수 있습니다.  


><img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정스토어-정보---구글-플레이.png" />


<br />

## Google Play RSA 공개키
Google Play 결제 및 구독 상품의 영수증 검증에 사용되는 RSA 공개키입니다.
- 영수증 검증 과정의 보안 강화를 위해 필요합니다.
- 경로: Google Play Console > Monetize with Play > Monetization setup > Google Play Billing > Manage notifications

![image](/img/docs/guide/base-serversetting/store_google1.png)
