---
description: "영수증 검증"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 영수증 검증
뒤끝에서는 영수증 검증을 통해 확실한 결제 인증을 합니다.  
영수증 검증 기능을 통해 유저에게 아이템을 지급하기 전 유저가 부정 이용을 통해 결제를 진행한 것은 아닌지 한 번 더 확인할 수 있습니다.  

<ConsoleLinkButton text="영수증 검증 바로가기" menu="baseReceipt" feature="영수증 검증" title="영수증 검증" />

관련 콘솔 가이드 : [뒤끝베이스 - 영수증 검증](/guide/console-guide/backnd-base/receipt)  
관련 SDK 개발자 문서 : [구글 영수증 검증](/sdk-docs/backend/base/receipt/receipt/android/setting-google-console) / [애플 영수증 검증](/sdk-docs/backend/base/receipt/receipt/ios/setting-google-console) / [원스토어 영수증 검증](/sdk-docs/backend/base/receipt/receipt/onestore/setting-onestore-console)

## 영수증 검증 지원
뒤끝은 **구글 플레이스토어**와 **애플 앱스토어**, **원스토어**의 결제 영수증에 대한 유효성 검증을 지원합니다.  
- **영수증 자체의 유효성**과 **구매한 ProductID**가 올바른지 검증합니다.  
- 한번 검증한 영수증을 다시 검증하려 할 경우 에러가 발생하여 결제 없이 아이템을 구매하려는 행위를 막을 수 있습니다.  

## 영수증 검증 내역 확인
뒤끝 콘솔에서 영수증 검증 내역을 확인할 수 있습니다.  
어떤 유저가 언제 영수증 검증을 시도했는지에 대해 자세히 확인할 수 있습니다.  
