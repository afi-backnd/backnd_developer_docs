---
description: "영수증 검증"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 영수증 검증

게임 내에서 [ChargeTBC](/sdk-docs/backend/base/cash/recharge-tbc) 혹은 [IsValidateGooglePurchase](/sdk-docs/backend/base/receipt/receipt/android/verify), [IsValidateApplePurchase](/sdk-docs/backend/base/receipt/receipt/ios/verify)을 통한 영수증 검증 내역을 확인할 수 있습니다.  

<ConsoleLinkButton text="영수증 검증 바로가기" menu="baseReceipt" feature="영수증 검증" title="영수증 검증" />

- 영수증 검증 기능 단독 이용시 금액 정보는 표시되지 않습니다.  
  - 영수증 검증 시 금액정보를 표기하기 위해서는 GetCheckGoogleReceiptWithPrice 를 통해 금액 정보를 입력해 주어야 합니다.
  - Google 스토어의 order id는 24년 4월 30일 이후의 결제 건만 확인 가능합니다. 

  ![image](/img/docs/guide/console-guide/backnd-base/orderid/receipt_01.png)
  ![image](/img/docs/guide/console-guide/backnd-base/orderid/receipt_02.png)

  - 원하는 화폐를 선택하여 확인할 수 있습니다.
  ![image](/img/docs/guide/console-guide/backnd-base/orderid/receipt_02-2.png)


## 스토어별 필수 입력 항목

영수증 검증을 이용하기 위해 스토어별 필수 항목을 입력해야 합니다.
- 미등록 항목이 있을 경우 영수증 검증 메뉴 진입 시 안내 팝업이 표시됩니다.  
- 각 항목을 클릭하면 등록 메뉴로 바로 이동할 수 있습니다.

![영수증 검증 필수 항목 안내 팝업](/img/docs/guide/console-guide/backnd-base/receipt/receipt_required.png)

각 항목은 아래 메뉴에서 등록할 수 있습니다.

- [인증정보 바로가기](/guide/console-guide/server-setting/authenciation)
- [구글 플레이 스토어 정보 바로가기](/guide/console-guide/server-setting/store/googlestore)
- [앱 스토어 스토어 정보 바로가기](/guide/console-guide/server-setting/store/appstore)
- [원스토어 스토어 정보 바로가기](/guide/console-guide/server-setting/store/onestore)

<br />

## 환불처리

영수증 검증 페이지에서는 개발자가 임의로 환불처리를 표시할 수 있습니다.  
해당 환불처리는 게임 및 결제에 어떠한 영향도 가지 않으며, 그저 운영중 편의를 위해 환불처리를 표시할 수 있도록 제공하는 기능입니다.  

- Google PlayStore 결제 상세 내용
  ![image](/img/docs/guide/console-guide/backnd-base/orderid/receipt_03.png)

- 원스토어, 앱스토어 결제 상세 내용
  ![image](/img/docs/guide/console-guide/backnd-base/orderid/receipt_04.png)
  ![image](/img/docs/guide/console-guide/backnd-base/orderid/receipt_05.png)


