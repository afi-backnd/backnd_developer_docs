import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 캐시 관리

게임 캐시 관리는 다음 조건 중 한 가지 이상을 충족했을 때 접근 가능합니다.  

- Android Package Name 입력과 JWT 토큰 업로드
- iOS Bundle Identifier 입력

:::info
영수증 검증만 사용하는 경우, <b>캐시 관리와 캐시 아이템 관리, 사용 내역</b>은 관리하지 않아도 됩니다.  
:::

<ConsoleLinkButton text="캐시 바로가기" menu="baseGameCash/cash" feature="캐시" title="캐시 관리" />

## 캐시 관리

TBC는 The Backend Coin의 약자로, 게임 내 캐시(재화)를 통칭하는 용어로서, **완전히 신뢰할 수 있음**을 목적으로 기획된 서버 재화입니다.  
TBC는 게임 내(뒤끝 SDK)에서는 영수증 검증을 통해서만 지급되며, 콘솔에서는 운영자가 특정 유저를 지정하여 지급/회수할 수 있습니다.  
영수증 검증을 통해서만 충전이 가능하고, 클라이언트에서 별도의 함수 호출을 통해서는 충전이 불가능합니다.  
게임 내에서 TBC 충전은 [ChargeTBC](/sdk-docs/backend/base/cash/recharge-tbc)를 통해서 할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/캐시 관리/뒤끝베이스--캐시-관리.png" />

### 사용방법 Tip

TBC는 영수증 검증 시점에서만 충전이 가능하며, 게임 내 보상(월 정액 보상, 랭킹 보상 등)으로 지급이 불가능합니다.  
게임 내 보상 혹은 정기적으로 지급해야 하는 재화의 경우 게임 **기획 단계에서부터 TBC와 별도의 재화를 두고** 사용하는 방식을 추천드립니다.  

TBC와 별도의 재화를 기획하였을 때

1. TBC와 별도의 재화는 합산 구매 불가능
2. TBC를 별도의 재화로 전환 가능
3. 별도의 재화를 TBC로 전환 불가능

위와 같이 TBC와 별도의 재화 간 가치를 1:1로 맞추면 게임 내 보상으로 TBC를 지급한 것과 동일한 효과를 가져올 수 있습니다.  

### 캐시 등록

캐시는 현금 금액이 TBC 얼마에 해당하는지 확인하여 TBC를 지급하기 위한 수단입니다.  
이 캐시의 제품 ID는 구글 혹은 iOS에 등록한 인앱 상품의 제품 ID와 같아야 TBC를 지급합니다.  
예를 들어, 아래의 캐시는 1,000원짜리 인앱 상품으로, TBC의 100에 해당하는 금액입니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/캐시 관리/뒤끝베이스--캐시-등록.png" />

### CSV 업로드

구글 플레이 콘솔에서 인앱 상품의 리스트를 CSV 파일로 받은 뒤, TBC column을 추가하여 각 상품에 해당하는 TBC 값을 입력 후, 뒤끝 콘솔에 업로드하면 자동으로 캐시 리스트를 생성합니다.  

**구글 플레이 콘솔 → 애플리케이션 선택 → 앱 정보 → 인앱 상품 → 관리되는 제품 → 가져오기/내보내기 → 관리되는 제품을 CSV 파일로 내보내기**를 통해 CSV 파일을 다운로드할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/console/gameCash/1-2.png" />

## 업로드 조건

- CSV에 포함되어야 하는 column 명
    - Product ID
    - TBC
    - Locale; Title; Description
    - Price
- 각 column 별 값 조건
    - **Locale; Title; Description** column의 row에는 "Locale;"에 해당하는 ko_KR; 값과 "Title;" 값이 있어야 함
    - **Price** column의 row에는 화폐단위 "KR;"이 있어야 함
    - 중복 Product ID는 업로드 불가
    - 인앱 상품이 가격 템플릿을 사용하는 경우, 업로드 불가
      > [예시 파일 다운로드](https://developer.thebackend.io/static/files/in_app_products.csv)

## 캐시 아이템 관리

캐시 아이템은 캐시(TBC)를 사용하여 구매할 수 있는 아이템입니다.  
해당 아이템은 게임 내에서 [GetProductList](/sdk-docs/backend/base/cash/search-cash-item)를 통해서 캐시 아이템 리스트를 받아온
뒤, [UseTBC](/sdk-docs/backend/base/cash/purchase-using-tbc)를 통해서 구매할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/캐시 관리/뒤끝베이스--캐시-아이템-등록.png" />

<div id="receipt" ></div>

## 거래 내역

게임 내에서 [ChargeTBC](/sdk-docs/backend/base/cash/search-cash-item)
혹은 [UseTBC](/sdk-docs/backend/base/cash/purchase-using-tbc)를 통한 TBC 구매/사용 또는 관리자 지급/회수 등의 내용을 확인할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/캐시 관리/뒤끝베이스--캐시---거래내역.png" />
