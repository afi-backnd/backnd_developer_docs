---
description: "결제 관리"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 결제 관리

## 결제 관리

뒤끝 상단(GNB)의 `프로필 → 드롭다운 메뉴 → 결제 관리` 메뉴에서 결제 관리를 하실 수 있습니다.

<ConsoleLinkButton text="결제 관리 바로가기" href="https://console.thebackend.io/account/payment" feature="결제 관리" title="결제 관리" />

![이미지 1](/img/docs/guide/billing/dropdown.png)

## 현재 요금

현재 요금에서는 이달과 지난달의 현재 이용 요금을 확인하실 수 있습니다.  
프로젝트별 이용 내역이 노출되고, 요금이 집계되는 항목은 아래와 같습니다.
- 뒤끝베이스 요금
- 푸시 요금
- 뒤끝펑션 요금
- 채팅 요금
- DB 요금

총합계와 예상 요금을 확인하실 수 있습니다.

![이미지 2](/img/docs/guide/billing/current-usage.png)
![이미지 3](/img/docs/guide/billing/usage-history.png)

### 요금 상세 내역

- 항목별 요금 상세 내역을 확인하실 수 있습니다.
- 사용량이 발생한 항목만 노출되며, 사용량이 0인 경우에는 노출되지 않습니다.

![이미지 4](/img/docs/guide/billing/fee-detailed-history.png)

## 청구 내역

- 월별 청구 내역을 확인하실 수 있습니다.
- 청구서는 익월 5일에 발행됩니다.
- 청구서가 발행되면 10일에 등록된 결제 정보로 자동 결제가 진행됩니다.

![이미지 5](/img/docs/guide/billing/billing.png)

:::info 요금 관련 유의사항 안내
- 요금은 UTC 기준으로 집계됩니다.  
예) 5월 1일(UTC) 요금은 한국 시간으로 5월 1일 09시 00분 ~ 5월 2일 8시 59분(KST)까지의 사용량을 기준으로 합니다. 
- 데이터 스토리지 사용량은 지속적으로 모니터링하여 집계되고 사용한 총 누적량과 인덱스 사용량을 기준으로 스토리지 요금을 결정합니다.
- 트랜잭션 쓰기와 트랜잭션 읽기의 각 처리량은 2처리량으로 책정됩니다.
- 데이터 스토리지와 백업은 1GB 단위로 요금이 책정됩니다.  
예) 데이터 스토리지 무료 사용량 5GB를 초과하면 초과 시점부터 6GB 이하까지의 사용량 구간에서 $0.0169의 요금이 청구됩니다.
- 콘솔에서 DAU, MAU 등 지표를 계산할 때에도 DB 읽기/쓰기 요금이 발생합니다.
- 자동 또는 수동으로 데이터가 삭제될 때에도 DB 읽기/쓰기 요금이 발생합니다.
- 뒤끝 펑션은 FREE 플랜에서도 사용량을 초과하면 추가 요금이 청구됩니다.
:::

## 결제 정보

결제 정보를 입력하여 등록하실 수 있습니다.

![이미지 6](/img/docs/guide/billing/payment-information.png)

- 등록된 결제 정보로 매월 10일에 자동 결제됩니다.
- 결제 정보 변경 버튼을 클릭해 등록된 결제 정보를 변경하실 수 있습니다.

![이미지 7](/img/docs/guide/billing/payment-information-save.png)

<!-- 결제 정보 영문 번역은 해당 내용으로 해주시기 바랍니다.

PayPal 버튼을 클릭하여 PayPal 정기 결제 수단을 등록하실 수 있습니다.

- 등록된 PayPal 결제 정보로 매월 10일에 자동 결제됩니다.
- PayPal의 등록, 변경 일자를 확인할 수 있습니다.
- 결제 수단 등록 시, 결제 수단의 유효성 확인을 위해 $0.00의 결제가 발생할 수 있습니다.
- 등록된 PayPal 계정을 수정하려면 상단의 PayPal 버튼을 클릭한 뒤, 새 계정으로 로그인하고 등록하면 수정이 완료됩니다. -->



