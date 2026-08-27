---
description: "대시보드"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 대시보드

대시보드를 통해 주요 게임 운영 지표(매출, DAU, 리텐션 등)를 실시간으로 확인하고 게임 운영을 효율적으로 관리하세요.  

<ConsoleLinkButton text="대시보드 바로가기" menu="dashboard/dashboard" feature="대시보드" title="대시보드" />

## 주요 지표

게임 운영 현황을 한눈에 파악할 수 있는 핵심 데이터입니다.  
주요 지표를 통해 유저 활동과 매출 현황을 종합적으로 분석하고, 서비스 운영 방향을 설정할 수 있습니다.  

### 기본 지표
기본 지표 데이터를 통해 게임의 성장 추이와 유저 활동 수준을 빠르게 파악할 수 있습니다.  

![](/img/docs/guide/dashboard/basicIndicator.png)

- **전체 매출:** 게임의 전체 누적 매출액입니다.
    - 스토어 간 매출 보고서 갱신 주기 차이로 인해 이틀 전까지의 누적 매출이 제공됩니다.
- **NRU (Newly Registered Users)**: 신규 가입 유저 수
- **ARU (All Registered Users)**: 전체 유저 수
- **총 실행 횟수**: 중복 포함 전체 초기화 실행 횟수
- **평균 실행 횟수**: 유저 1인당 평균 실행 횟수
- **신규 유저 비율**: 일간 활성 유저(DAU) 대비 신규 유저 비율

*매출 관련 데이터(전체 매출, 일별 매출, ARPDAU, ARPPU 등)는 대시보드 화면 우측 상단 드롭다운 메뉴에서 선택한 화폐 단위로 표시됩니다.  

### 매출 지표

게임의 수익성과 결제 패턴을 분석할 수 있는 지표입니다.  
해당 지표를 통해 매출 구조를 확인하고, 서비스 운영 및 BM 전략 개선에 활용하세요.  

![](/img/docs/guide/dashboard/sales.png)

- **매출**: 해당 월의 일자별 매출을 확인할 수 있습니다.  
    - 스토어 간 매출 보고서 갱신 주기 차이로 인해 이틀 전까지의 매출 정보가 제공됩니다.  

![](/img/docs/guide/dashboard/pu.png)

- **PU (Paying Users)**: 결제 유저 수 *(뒤끝 영수증 검증 사용 시 제공)*
- **PUR (Paying User Ratio)**: 일간 활성 유저(DAU) 대비 결제 유저 비율  

![](/img/docs/guide/dashboard/paymentCount.png)

- **주문 수**: 발생한 결제 주문 수 *(뒤끝 영수증 검증 사용 시 제공)*
- **평균 주문율**: 유저가 평균적으로 주문하는 비율  

![](/img/docs/guide/dashboard/arpdau.png)

- **ARPDAU (Average Revenue Per DAU)**: 일간 활성 유저(DAU) 기준 평균 매출액
- **ARPPU (Average Revenue Per Paying User)**: 결제 유저 1인당 평균 매출액  

## 활성 사용자

유저의 실제 활동 규모를 보여주는 지표입니다.  
DAU, MAU, Stickiness 데이터를 통해 이용 패턴을 분석하고, 게임 운영 상태를 지속적으로 관리하세요.  

![](/img/docs/guide/dashboard/dau.png)

- **DAU (Daily Active Users)**: 일간 활성 사용자 수  

![](/img/docs/guide/dashboard/mau.png)

- **MAU (Monthly Active Users)**: 월간 활성 사용자 수  

![](/img/docs/guide/dashboard/stickiness.png)

- **Stickiness**: 고착률 (MAU 대비 DAU 비율)

## 리텐션

리텐션을 확인하여 유저의 재방문 패턴을 분석하고, 게임 운영 전략을 최적화하세요.  

![](/img/docs/guide/dashboard/retention.png)

- 데이터는 백분율(%) 또는 수치로 제공됩니다.
- 특정 일자를 선택하여 해당일 신규 유저의 리텐션을 확인할 수 있습니다.

:::tip 멀티 캐릭터를 사용 중인가요?
멀티 캐릭터 프로젝트에서는 유저 수 관련 지표(주요 지표 · 활성 사용자 · 리텐션)가 **계정 기준**으로 집계됩니다. [보기](/guide/console-guide/backnd-base/users/multi-character#다른-메뉴에서의-동작)
:::
