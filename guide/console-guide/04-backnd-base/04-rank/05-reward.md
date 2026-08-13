---
sidebar_label: "보상"
sidebar_position: "5"
description: "보상"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 보상
리더보드가 초기화될 때 최종 순위에 따라 참여 대상에게 [우편](/guide/getting-started/how-to-use/post)으로 보상을 지급할 수 있습니다.

<ConsoleLinkButton text="보상 바로가기" menu="baseRank/reward" feature="랭킹" title="랭킹 보상" />

## 사전 작업
보상으로 지정할 수 있는 데이터가 있어야 합니다.  
이 데이터를 차트(데이터 테이블)에 올려주세요.
1. [차트 메뉴](/guide/console-guide/backnd-base/chart)에서 차트를 생성합니다.
2. 차트 파일 (.csv, .xlsv 등)을 업로드합니다.

## 보상 생성하기
'보상' 탭에서 '보상 생성하기' 버튼을 통해 순위에 따른 보상을 설정할 수 있습니다.
* '보상 구간'은 동일한 보상을 받는 순위 구간입니다.
* 보상 구간은 최대 20개 생성할 수 있습니다.  
* 한 구간 당 최대 3,000개의 순위를 포함할 수 있습니다.  
* 최대 10,000등까지 입력할 수 있습니다.  
* 전체 보상을 1개 설정할 수 있습니다.  
  - 전체 보상은 최대 10,000등까지 보상을 설정할 수 있는 것과 별개로, 참여한 모든 유저에게 지급되는 보상입니다.
  - 따라서 리더보드 초기화에 따른 보상은 전체 보상을 포함하여 최대 2개의 보상을 수령할 수 있습니다.  
* 리더보드 보상에 사용한 차트가 변경되었을 경우 리더보드 보상 또한 함께 수정되어야 합니다.  

## 보상 우편
- 발송된 랭킹 보상의 만료기간은 랭킹이 종료 또는 갱신된 시점부터 일주일입니다.  
- 랭킹 보상 발송 내역에 대해서는 [우편 발송 > 랭킹 보상](/guide/console-guide/backnd-base/post/reward-rank) 문서를 확인해 주세요.
