---
description: "신고 및 제재"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 신고 및 제재

신고 내역을 확인하고 유저에게 제재를 가할 수 있습니다.

<ConsoleLinkButton text="신고 및 제재 바로가기" menu="chatReport" feature="신고 및 제재" title="신고 및 제재" />

## 신고 목록

신고와 관련한 정보를 한눈에 확인할 수 있습니다.

![reports list](/img/docs/guide/chat/console/rnr-list.png)

- 신고 내역은 최초 신고일을 기준으로 내림차순 정렬되어 보여집니다.
- 신고일은 최초 신고일로 보여집니다.
- 유저 닉네임을 호버하면 UUID를 확인할 수 있습니다.
- 신고한 유저가 여러 명일 경우 최초 신고자 외 N 명으로 보여집니다.
- 신고 사유가 여러 개일 경우 최초 신고 사유 외 N 개로 보여집니다.
- 신고의 상태는 처리 전 / 처리 완료 2가지로 나뉩니다.
- 유저의 처리 상태는 유저 제재 메뉴에서 확인할 수 있습니다.

### 검색

- 신고된 유저 UUID, 신고한 유저 UUID, 신고일 기준으로 정확히 일치하는(equals) 항목을 검색할 수 있으며, 신고된 유저 닉네임, 신고한 유저 닉네임은 중간자(includes) 검색이 가능합니다.
- 검색 기간은 최대 7일 이내로 설정할 수 있습니다.

### 필터

속성, 연산자, 값을 설정하여 필터를 적용할 수 있습니다.

- 속성은 신고일, 신고된 유저 닉네임, 신고된 유저 UUID, 신고한 유저 닉네임, 신고한 유저 UUID, 신고 사유 Key, 상태, 처리 방법을 제공합니다.

![reports filter](/img/docs/guide/chat/console/rnr-filter.png)

### 오늘 신고 수

오늘 접수된 신고 수를 보여줍니다. 제목을 클릭하면 ‘신고일 - 오늘’ 필터가 적용된 목록을 볼 수 있습니다.

![reports today](/img/docs/guide/chat/console/rnr-today.png)

### 일괄 처리하기

1개 이상의 신고 내역을 선택하여, 신고 당한 유저를 일괄적으로 처리할 수 있습니다.

아직 처리되지 않은 건과 처리된 건을 분리하여 선택할 수 있고, 제재 안 함 / 채팅 금지 / 이용 정지 / 영구 정지 중에 선택하여 유저에게 제재를 가할 수 있습니다.

![restrict multiple users](/img/docs/guide/chat/console/rnr-restrict-multiple.png)

## 신고 상세 페이지

신고된 메시지, 신고된 유저 및 신고한 유저, 신고일, 신고 사유, 처리 방법 등 신고에 대한 상세 정보를 볼 수 있는 페이지입니다.

![reports detail](/img/docs/guide/chat/console/rnr-detail.png)

### 메시지 숨기기

[메시지 숨기기] 버튼을 클릭하면 메시지 숨김 방법을 선택하여 메시지를 숨김 처리할 수 있습니다.

- 메시지를 삭제하면 채널 내에 있는 유저들은 메시지를 볼 수 없게 됩니다. 관리자는 원본 내용을 콘솔에서 확인할 수 있습니다.
- 메시지를 `*`로 대체하면 채널 내에 있는 유저들에게는 해당 메시지 내용이 전부 `*`로 대체된 상태로 노출됩니다. 관리자는 원본 내용을 콘솔에서 확인할 수 있습니다.

![hide message](/img/docs/guide/chat/console/rnr-detail-hide.png)

### 채널 바로 가기

메시지가 속한 채널을 클릭하면 해당 채널의 상세 페이지 - 채팅 로그로 이동합니다.

![go channel](/img/docs/guide/chat/console/rnr-detail-go-channel.png)

### 누적 신고 수

누적 신고 수를 클릭하면 신고된 유저 필터가 적용된 신고 내역 페이지로 이동합니다.

![filter by user](/img/docs/guide/chat/console/rnr-detail-go-user.png)

### 처리하기

아직 처리되지 않은 건과 처리된 건을 분리하여 선택할 수 있고, 제재 안 함 / 채팅 금지 / 이용 정지 / 영구 정지 중에 선택하여 유저에게 제재를 가할 수 있습니다.

![restrict a user](/img/docs/guide/chat/console/rnr-detail-restrict.png)

이미 처리된 유저의 [처리 상태 변경하기]를 클릭하여 변경하면, 마지막으로 변경한 제재 상태를 적용합니다.

## 제재 목록

채팅 금지가 적용된 유저의 목록을 확인하고, 제재를 가하거나 제재를 해제할 수 있습니다.

![ban chat list](/img/docs/guide/chat/console/rnr-ban-chat-list.png)


## 권한에 따른 닉네임 노출
- 닉네임 노출 권한이 없는 운영자는 유저의 닉네임이 마스킹(**)되어 확인할 수 없으며, 검색 대상에도 포함되지 않습니다.
- 닉네임 노출 권한은 '프로필 > 관리자 계정 관리 > 역할 > 채팅 탭'에서 설정할 수 있습니다.
