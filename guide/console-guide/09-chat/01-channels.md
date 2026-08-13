---
description: "채널"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 채널

채널 페이지에서 채널 목록과 메시지 내역을 확인하고 관리할 수 있습니다

<ConsoleLinkButton text="채널 바로가기" menu="chatChannel" feature="채널" title="채널" />

## 채널 목록

유저는 채널 안에서 메시지를 주고받습니다. 채널 메뉴에서는 채팅이 이루어지고 있는 채널들을 채널 그룹별로 확인할 수 있습니다.

![channel list](/img/docs/guide/chat/console/channel-list.png)

### 채널

채널은 3개의 종류로 나뉩니다.

- 프라이빗 채널
  - 최대 인원이 설정된 값으로 고정됩니다.
  - 유저가 채널을 생성할 수 있습니다. 유저가 채널을 생성하는 메소드는 [채팅 SDK 문서](/sdk-docs/chat/channel)에서 확인하세요.
  - 채널에 비밀번호를 설정할 수 있습니다.
- 오픈 채널
  - 인원에 따라서 채널이 자동으로 확장됩니다. 확장된 채널은 1, 2, 3, ... 순서대로 채널 번호가 부여됩니다.
  - 채팅 서버에 접속할 때 채널에 자동으로 입장합니다.
  - FALLBACK 채널을 지정할 수 있습니다. 입장해야 하는 특정 채널이 없는 경우, 자동으로 FALLBACK 채널에 입장합니다.
- 예약어 채널
  - 귓속말, (베이스의 길드 기능과 연동된) 길드별 채널, 언어별 채널, 국가별 채널, 국가코드별 채널을 사용할 수 있습니다.
  - 채널 그룹 관리 페이지에서 사용 여부를 설정할 수 있습니다.

### 채널그룹 및 채널 생성하기

채널 그룹 생성하기 버튼을 클릭하면 생성하고자 하는 채널의 타입을 선택할 수 있습니다.

![channel empty](/img/docs/guide/chat/console/channel-empty.png)
![channel type](/img/docs/guide/chat/console/channel-create-type.png)

#### 프라이빗 채널

프라이빗 타입을 선택한 경우, 조건에 맞춰 채널 그룹 이름을 입력합니다.

- 프라이빗 채널 그룹 이름은 영어 소문자, 숫자, 또는 '-'를 사용하여 4byte 이상 48byte 이하로 입력할 수 있습니다.
- `language`, `country`, `countrycode`, `guild`, `whisper` 는 채널 그룹 이름으로 사용할 수 없습니다.

![create private channel group](/img/docs/guide/chat/console/channel-create-private-channel-group.png)

채널 정보를 입력합니다.

- 프라이빗 채널 이름은 4byte 이상 48byte 이하의 아무 문자를 입력할 수 있습니다.
- 최대 인원은 최소 2부터 요금제에서 선택한 채널 최대 인원 값까지 입력할 수 있습니다.
- 채널에 비밀번호를 설정할 수 있습니다.

![create private channel](/img/docs/guide/chat/console/channel-create-private-channel.png)

생성하기를 클릭하면 채널그룹과 채널이 만들어집니다.

- 비밀번호가 설정된 채널은 이름 옆에 열쇠 아이콘이 노출됩니다.

![created private channel](/img/docs/guide/chat/console/channel-created-private.png)

#### 오픈 채널

오픈 타입을 선택한 경우, 조건에 맞춰 채널 그룹 정보를 입력합니다.

- 오픈 채널 그룹 이름은 영어 소문자, 숫자, 또는 '-'를 사용하여 4byte 이상 48byte 이하로 입력할 수 있습니다.
- `language`, `country`, `countrycode`, `guild`, `whisper` 는 채널 그룹 이름으로 사용할 수 없습니다.
- 채널 당 최대 인원은 모든 채널에 적용됩니다. 최소 2부터 요금제에서 선택한 채널 최대 인원 값까지 입력할 수 있습니다.
- FALLBACK 채널을 설정할 수 있습니다. 이 기능을 활성화하는 경우 첫번째 채널이 FALLBACK으로 설정됩니다.

![create open channel group](/img/docs/guide/chat/console/channel-create-open-channel-group.png)

채널 이름을 입력합니다.

- 영어 소문자, 숫자, 또는 '-'를 사용하여 4byte 이상 48byte 이하로 입력할 수 있습니다.

![create open channel](/img/docs/guide/chat/console/channel-create-open-channel.png)

생성하기를 클릭하면 채널그룹과 채널이 만들어집니다.

- FALLBACK으로 설정된 채널은 이름 옆에 FALLBACK 뱃지가 노출됩니다.

![created open channel](/img/docs/guide/chat/console/channel-created-open.png)

#### 예약어 채널

채널 그룹 관리 페이지에서 귓속말, 길드별 채널, 언어별 채널, 국가별 채널, 국가코드별 채널의 사용 여부를 설정할 수 있습니다.

:::caution

길드별 채널은 베이스 SDK의 ‘길드’ 기능을 사용 중일 경우에만 유효한 기능입니다.

:::

![channel group settings](/img/docs/guide/chat/console/channel-group-settings.png)

언어별 채널을 사용할 경우, 활성화할 채널과 채널당 최대 인원을 설정해야 합니다.

- 유저의 `language` 속성을 바탕으로 나올 수 있는 모든 옵션을 제공합니다.
- 해당 속성을 가진 유저가 존재하는지(존재하는 유저 수)를 확인하고, 활성화할 채널을 선택하세요.
- FALLBACK 버튼 영역을 클릭하면, 해당 채널이 FALLBACK으로 변경됩니다.

![auto channel group](/img/docs/guide/chat/console/channel-group-auto.png)

### 채널 선택

(오픈 채널일 경우) 선택한 채널을 FALLBACK 채널로 설정하거나, 관리자 메시지를 전송하거나, 삭제할 수 있습니다.

- FALLBACK 채널이 아닌 1개의 채널을 선택하여 FALLBACK 채널로 지정할 수 있습니다.
- FALLBACK 채널은 삭제할 수 없습니다.
- 선택된 모든 채널에 일괄적으로 관리자 메시지(`system message`)를 전송할 수 있습니다.

![select channels](/img/docs/guide/chat/console/channel-bulk-actions.png)
![send system message](/img/docs/guide/chat/console/channel-system-message.png)

### 채널 검색

중간자 검색(contains) 방식으로 채널을 검색할 수 있습니다.

![search channel](/img/docs/guide/chat/console/channel-search.png)

### 채널 필터

속성, 연산자, 값을 설정하여 필터를 적용할 수 있습니다.

![filter channel](/img/docs/guide/chat/console/channel-filter.png)

### 채널 정렬

테이블 헤더 항목을 클릭하여 채널 정렬 기준을 변경할 수 있습니다.

![sort channel](/img/docs/guide/chat/console/channel-sort.png)

### 채널 그룹 수정

오픈 타입의 채널 그룹 설정을 수정할 수 있습니다.
:::caution

- 변경된 설정의 영향을 받는 유저들 중, 이미 접속되어 있는 유저들은 서버에 재접속하는 시점에 변경사항이 적용됩니다.
- 예를 들어 FALLBACK 채널이 A 채널에서 B 채널로 변경된 경우, 기존 FALLBACK 로직에 따라 A 채널에 입장해있는 유저들은 A 채널에 남아있다가, 서버에 재접속하는 시점에 B 채널로 입장합니다.

:::

### 채널 그룹 삭제

채널 그룹을 삭제하는 경우, 속해있던 모든 채널이 삭제됩니다. 유저들은 해당 채널에서 더 이상 채팅을 할 수 없게 됩니다.

## 채널 상세

채널을 클릭하면 채널 상세 페이지가 열립니다.  
채널 내에서 이루어진 대화를 **채팅 로그** 형식 또는 **채팅방** 형식으로 확인할 수 있습니다.
:::info

채널 상세 페이지에서 대화 내역을 보려면 [설정](/guide/console-guide/chat/settings#서버)에서 부가 기능을 활성화해야 합니다.

:::

![channel detail](/img/docs/guide/chat/console/channel-detail.png)

### 채팅 로그

- 채팅은 시간 기준 내림차순으로 정렬되며, 최근 50개 메시지가 로드됩니다.
- **새로고침 버튼**을 클릭하면 테이블이 갱신됩니다.
- 이전 내역이 있는 경우, **[이전 메시지 불러오기]** 버튼을 클릭하면 이전 50개 메시지가 추가로 로드됩니다.

![channel load](/img/docs/guide/chat/console/channel-detail-load-previous-messages.png)

### 채팅 로그 상세 모달

채팅 로그의 행을 클릭하면 채팅 메시지 상세 모달이 표시됩니다. 해당 메시지와 함께 이전·이후 5분간의 메시지도 확인할 수 있습니다.  
'메타 데이터 출력'을 체크하면 각 유저의 [메타데이터](/sdk-docs/chat/user/)를 볼 수 있습니다.(메시지 전송 시점 기준의 메타데이터 출력)

![channel detail modal](/img/docs/guide/chat/console/channel-detail-chat-modal.png)

### 채팅 로그 검색

유저의 UID, 닉네임 또는 채팅 내용을 검색할 수 있으며, 검색 기간은 최대 1시간 이하로 설정할 수 있습니다.

![channel detail search](/img/docs/guide/chat/console/channel-detail-search.png)

### 메시지 숨기기

메시지를 호버하면 메시지 숨기기 버튼이 나옵니다. 이 버튼을 클릭하여 메시지를 숨김 처리할 수 있습니다. 메시지를 숨김 처리하면 유저에게 메시지 내용이 보이지 않게 됩니다.

![channel detail hide](/img/docs/guide/chat/console/channel-detail-hide.png)
![channel detail hide modal](/img/docs/guide/chat/console/channel-detail-hide-modal.png)
![channel detail hidden](/img/docs/guide/chat/console/channel-detail-hidden.png)

### 유저 제재하기

메시지를 호버하면 유저 제재하기 버튼이 나옵니다. 이 버튼을 클릭하여 원하는 방식으로 유저를 제재할 수 있습니다.

![channel detail sanction](/img/docs/guide/chat/console/channel-detail-sanction.png)
![channel detail sanction modal](/img/docs/guide/chat/console/channel-detail-sanction-modal.png)

### 관리자 메시지 보내기

관리자 메시지 보내기 버튼을 클릭한 후 텍스트를 입력하고 전송하면, 해당 채널에 실시간으로 관리자 메시지를 전송할 수 있습니다.

![channel detail system message](/img/docs/guide/chat/console/channel-detail-system-message.png)

### 채팅방

- 채팅방에서 실시간으로 최신 메시지를 확인할 수 있습니다.
- 하단 입력창을 통해 관리자 메시지를 입력하고 전송할 수 있습니다.
- 메시지에 호버하면 "메시지 숨기기" 및 "유저 제재" 버튼이 표시됩니다. 이를 클릭하여 채팅방에서도 유저를 제재할 수 있습니다.

![channel detail sanction](/img/docs/guide/chat/console/channel-detail-chatroom-system-message.png)
![channel detail sanction modal](/img/docs/guide/chat/console/channel-detail-chatroom-hide.png)


## 권한에 따른 닉네임 노출
- 닉네임 노출 권한이 없는 운영자는 유저의 닉네임이 마스킹(**)되어 확인할 수 없으며, 검색 대상에도 포함되지 않습니다.
- 닉네임 노출 권한은 '프로필 > 관리자 계정 관리 > 역할 > 채팅 탭'에서 설정할 수 있습니다.
