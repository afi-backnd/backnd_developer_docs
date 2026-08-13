---
description: "구글 플레이 스토어(구버전 콘솔)"
---

# 구글 플레이 스토어(구버전 콘솔)

## 구글 플레이 정보 입력

다음 정보는 영수증 검증 기능 이용 및 대시보드의 매출 관련 지표를 제공하기 위해 필요한 정보입니다.  
해당 정보를 입력 시, 뒤끝 콘솔의 영수증 검증 기능 및 대시보드의 매출 관련 지표 정보를 이용할 수 있습니다.  
해당 정보는 [구글 플레이 콘솔](https://play.google.com/apps/publish/)에서 획득할 수 있습니다.  

- JWT(Json Web Token) JSON 파일
- 보고서 저장 버킷 URL

:::danger '버킷 URL이 올바르지 않습니다.' 메시지가 표시되는 경우 확인해야 할 사항
* 뒤끝 콘솔에서 서버 설정 > 인증정보 > Android Package Name이 올바르게 입력되었는지 확인하세요.  
* JWT 토큰의 권한 설정을 확인하세요.  
* 버킷 URL을 올바르게 설정했는지 확인하세요.  
:::

:::caution 버킷 URL이 존재하지 않는 경우
* <a href="#url2">버킷 URL</a>이 존재하지 않는 경우, 실 결제를 통해 결제 보고서를 생성한 후 url을 확인할 수 있습니다.  
:::
  


<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정스토어-정보---구글-플레이---구버전.png" />

## JWT(Json Web Token) JSON 파일 얻기

### 1. 구글 API 엑세스 프로젝트 생성

<img src="https://developer.thebackend.io/static/img/console/store/google/1-1.png" />

  


<img src="https://developer.thebackend.io/static/img/unity/receipt_and/2.png" />

### 2. 구글 서비스 계정 생성, JWT 다운로드

프로젝트에 연결된 것을 확인하고, JWT를 위한 서비스 계정을 생성합니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_and/3.png" />

구글 API 콘솔에 접속하며, 서비스 계정을 생성합니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_and/4.png" />

  


<img src="https://developer.thebackend.io/static/img/unity/receipt_and/5.png" />

서비스 계정의 비공개키 json 파일을 생성하면, 자동으로 json 파일이 다운됩니다.  
다운로드한 json 파일을 **뒤끝 콘솔 → 서버 설정 → 스토어 정보 → 구글 플레이 → JWT JSON 파일**에 업로드합니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_and/6.png" />

### 3.서비스 계정 액세스 권한 부여

위에서 생성한 서비스 계정의 액세스 권한을 부여(혹은 수정) 합니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_and/7.png" />

전체 게임의 **재무 데이터 보기**를 필수로 선택한 후, 저장합니다.  

<img src="https://developer.thebackend.io/static/img/console/store/google/1-8.png" />

## 보고서 저장 버킷 URL

<img src="https://developer.thebackend.io/static/img/console/store/google/2-1.png" />

  
<div id="url2"></div>

<img src="https://developer.thebackend.io/static/img/console/store/google/2-2.png" />

`pubsite_prod_rev_xxxxxxxxxxxx` 부분을 복사하여 **뒤끝 콘솔 → 서버 설정 → 스토어 정보 → 구글 플레이 → 보고서 저장 버킷 URL**에 등록합니다.  

> 이때 `gs://`는 **제외하고** 복사해야 합니다.  
> 버킷 URL이 존재하지 않는 경우, 실결제를 통해 결제보고서를 생성한 후 url을 확인할 수 있습니다.  


<img src="https://developer.thebackend.io/static/img/console/store/google/2-3.png" />
