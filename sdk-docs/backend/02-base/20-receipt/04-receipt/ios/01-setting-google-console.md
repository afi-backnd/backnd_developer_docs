---
sidebar_label: "애플 결제 콘솔 설정"
description: "애플 결제 콘솔 설정"
---

# 애플 결제 콘솔 설정

iOS 환경에서 뒤끝을 이용하여 애플 결제 영수증의 유효성 검증을 위해서는 애플 콘솔에서 아래와 같은 설정을 해야 합니다.  

### 1. Apple App IDs 설정

영수증을 검증하기 위해서는 [App IDs](https://developer.apple.com/account/ios/identifier/bundle)에 In-App Purchase가 설정되어 있어야 합니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_ios/1-1.png" />
<img src="https://developer.thebackend.io/static/img/unity/receipt_ios/1-2.png" />

<p> <img src="https://developer.thebackend.io/static/img/unity/receipt_ios/1-3.png" /></p>

### 2. [아이튠즈 커넥트](https://itunesconnect.apple.com/) 설정하기

신규 앱을 생성합니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_ios/2-1.png" />

필요한 값을 입력합니다. 번들 ID는 Apple Developer 페이지에서 App IDs를 설정했다면, 선택 가능한 항목이 자동으로 추가됩니다.  

<p> <img src="https://developer.thebackend.io/static/img/unity/receipt_ios/2-2.png"/></p>

이제 인앱결제를 할 상품을 등록합니다.  

<p> <img src="https://developer.thebackend.io/static/img/unity/receipt_ios/2-3.png" /></p>

상품의 성격에 따라 소모품, 비소모품, 자동 갱신 구독, 비 자동 갱신 구독으로 나뉩니다. 테스트하는 과정이므로 소모품만 진행하도록 하겠습니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_ios/2-4.png" />

상품 등록에 필요한 부분을 입력합니다.  

<p> <img src="https://developer.thebackend.io/static/img/unity/receipt_ios/2-5.png" /></p>

### 3. 테스터 등록하기

실제 앱을 마켓에 올리기 전에 테스트를 하기 위한 테스트(SandBox) 계정을 생성합니다. 사용자 및 역할로 들어와 Sandbox 테스터에 테스터를 등록합니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_ios/2-6.png" />

이 테스터는 실제 앱스토어에 등록되어 있는 계정이 아닙니다.  

<img src="https://developer.thebackend.io/static/img/unity/receipt_ios/2-7.png" />

테스트를 진행하기 전에 테스트 기기에서 AppStore에 연결되어 있는 계정을 로그아웃 시켜줍니다.  
Sandbox 테스터 아이디로 로그인을 합니다.  

> 테스트를 진행하기 전에 테스트 기기에서 AppStore에 연결되어 있는 계정을 로그아웃 시켜줍니다.  
> Sandbox 테스터 아이디로 로그인을 합니다.  
