---
sidebar_position: "2"
description: "환경설정"
---

# 환경설정

환경설정은 환불 관리 메뉴 우측 상단의 **[환경설정]** 버튼을 통해 진입할 수 있습니다.

![환경설정 진입](/img/docs/guide/refund/refund5.png)

환불 기능 사용을 위한 요금제 설정, 스토어별 웹훅 등록, 알림 자동 발송 및 자동 유저 차단을 설정할 수 있습니다.

![환경설정 화면](/img/docs/guide/refund/refund6.png)

## 요금제

환불 기능은 **BASIC**과 **PRO** 두 가지 요금제를 제공합니다 **[[자세히 보기]](https://backnd.com/ko/pricing/refund/)**

- 요금제 변경은 **즉시 적용**됩니다.
- 상위 요금제에서 하위 요금제로 변경한 경우, 변경 후 **30일간** 재변경이 제한됩니다.  
  (PRO → BASIC 또는 사용 안 함, BASIC → 사용 안 함)
- 기본료는 변경 시점을 기준으로 **일할 계산**됩니다.
- 요금제 변경 예약은 지원되지 않습니다.

:::info 무료 체험 기간
구독 시작 후 **7일간** 무료로 이용할 수 있습니다.  
무료 체험 기간 중에는 요금제 재변경 30일 제한이 적용되지 않습니다.  
무료 이용 기간 동안에는 ‘결제 관리 > 현재 요금 > 환불관리’의 사용 내역이 없음으로 표기됩니다.
:::


<br />


## 웹훅 등록

환불 기능은 각 스토어의 환불 완료 웹훅을 수신하여 데이터를 저장하므로 **스토어별 웹훅 등록이 필수**입니다.  
아래 정보를 참고하여 웹훅을 등록해 주세요.

:::warning 영수증 검증 연동 필요
스토어별 웹훅 등록 전, 해당 스토어에 대한 **영수증 검증** 연동이 완료되어야 합니다.  
영수증 검증이 연동되지 않은 스토어는 웹훅 정보가 표시되지 않습니다.  
영수증 검증 설정은 [영수증 검증](/guide/console-guide/backnd-base/receipt#스토어별-필수-입력-항목)을 참고해 주세요.
:::

<br />

### Google Play

구글 플레이 콘솔에 아래의 웹훅 토픽 정보를 등록합니다.

```
projects/thebackend-game-log/topics/iap-event-publisher-production
```

![구글플레이 화면](/img/docs/guide/refund/refund7.png)

- 경로: Google Play Console > Monetize with Play > Monetization setup > Google Play Billing > Manage notifications
- Notification content의 경우 **Subscriptions and voided purchases only**을 선택합니다.


<br />

### App Store

App Store Connect에 아래의 웹훅 URL을 등록합니다.

```
https://webhook.backnd.com/iap/apple/{Client UUID}
```

![앱스토어 화면](/img/docs/guide/refund/refund8.png)

- 경로: App Store Connect > App Information > App Store Server Notifications
- URL의 `{Client UUID}`는 환경설정 화면에서 확인할 수 있습니다.

<br />

### ONE Store

ONE Store 개발자 콘솔에 아래의 웹훅 URL을 등록합니다.

```
https://webhook.backnd.com/iap/onestore/{Client UUID}
```

![원스토어 화면](/img/docs/guide/refund/refund9.png)

- 경로: ONE Store 개발자 콘솔 > Apps > In-app Products(In-App 정보) > Managed product(관리상품) > Managed PNS
- URL의 `{Client UUID}`는 환경설정 화면에서 확인할 수 있습니다.


<br />

## 환불 알림 자동 발송

환불 완료 웹훅 수신 시 해당 유저의 클라이언트로 회수 알림을 자동으로 발송합니다.  
SDK에서 알림을 수신하는 방법은 [SDK 문서 - 환불 실시간 알림 핸들러](https://docs.backnd.com/sdk-docs/backend/base/notify/handler-user/post)를 참고해 주세요.

:::info PRO 전용 / 베이스 인증 유저 전용
환불 알림 자동 발송은 **PRO 요금제** 및 **베이스 인증 유저**에게만 지원됩니다.
:::


<br />

## 자동 유저 차단

환불 완료 웹훅 수신 시 해당 유저 UUID를 자동으로 차단합니다.  
- 차단 시 적용할 **차단 기간**과 **차단 사유**를 스토어별로 설정할 수 있습니다.
- **차단 제외** 등록된 주문의 경우 자동 차단 대상에서 제외됩니다.
- 스토어별로 개별 설정할 수 있으며, 기본값은 **사용 안 함**입니다.

![자동 유저 차단](/img/docs/guide/refund/refund10.png)

:::info PRO 전용 / 베이스 인증 유저 전용
자동 유저 차단은 **PRO 요금제** 및 **베이스 인증 유저**에게만 지원됩니다.
:::
