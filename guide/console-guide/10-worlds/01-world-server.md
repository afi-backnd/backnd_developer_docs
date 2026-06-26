---
title: 월드 서버
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 월드 서버

대규모 멀티플레이어 환경에서 데이터가 실시간으로 동기화되는 **월드 서버**를 생성하고 관리하세요.

<ConsoleLinkButton text="월드 바로가기" menu="worldWorldServer" feature="월드" title="월드" />
<br/>

## 월드 서버 생성하기

월드 서버 이름을 입력하여 월드 서버를 생성할 수 있습니다.

:::info
베타 기간 동안에는 리전(Seoul), 최대 CCU(100)으로 고정되며, **모든 기능이 무료로 제공**됩니다.
:::

![world server 1](/img/docs/guide/world/world-server/create.png)

## 월드 서버 목록

목록에서 생성된 월드 서버의 이름, UUID, 리전, 시뮬레이션 서버 상태를 확인할 수 있습니다.

![world server 2](/img/docs/guide/world/world-server/list.png)

## 월드 서버 상세 설정

월드 서버 상세 페이지에서는 시뮬레이션 서버 관리, 리모트 컨피그 설정, 데이터 테이블 연결 등 다양한 기능을 관리할 수 있습니다.

![world server 3](/img/docs/guide/world/world-server/detail.png)

### 시뮬레이션 서버

#### 상태
- `Stopped`: 시뮬레이션 서버가 중지된 상태이며, 시작 또는 빌드 파일 업로드를 통해 상태를 조작할 수 있습니다.
- `Starting`: 시뮬레이션 서버가 시작 중인 상태이며, 상태를 조작할 수 없습니다.
- `Running`: 시뮬레이션 서버가 정상적으로 돌아가고 있는 상태이며, 중지, 재시작, 빌드 파일 업로드를 통해 상태를 조작할 수 있습니다.
- `Error`: 에러가 발생하여 중단된 상태이며, 빌드 파일을 다시 업로드하세요.

#### 빌드 파일 업로드
- Linux 빌드 파일을 업로드하여 직접 구현한 서버 로직을 적용할 수 있습니다. [빌드 파일 업로드 방법](/sdk-docs/worlds/welcome)
- 빌드 파일을 업로드하면 업로드 - 빌드 - 배포 과정을 거쳐 시뮬레이션 서버에 적용됩니다.

![world server 5](/img/docs/guide/world/world-server/simulation-state-running.png)

### 리모트 컨피그

월드 서버에 실시간으로 반영되는 환경 변수를 설정할 수 있습니다.
- 시뮬레이션 서버가 `Running` 상태일 때: Value를 수정할 수 있습니다.
- 시뮬레이션 서버가 `Stopped` 또는 `Error` 상태일 때: 추가, 수정, 삭제 등 모든 종류의 수정이 가능합니다.
- 시뮬레이션 서버가 `Starting` 상태일 때: 수정이 불가합니다.

![world server 6](/img/docs/guide/world/world-server/remote-config.png)

#### Key
- 필수 입력 항목이며, 48bytes 까지 입력 가능합니다.
- 중복 입력은 불가합니다.

<!-- ![world server 7](/img/docs/guide/world/world-server/remote-config-key.png) -->

#### Type
- 필수 선택 항목이며, 총 9개의 타입을 제공합니다.
- `int32` / `uint32` / `int64` / `uint64` / `float` / `bool` / `json` / `string` / `datetime`

<!-- ![world server 6](/img/docs/guide/world/world-server/remote-config-type.png) -->

#### Value
선택 입력 항목이며, Type별 입력 제한을 확인하세요.

| Type | 설명 | 최솟값 | 최댓값 |
| --- | --- | --- | --- |
| int32 | 32-bit integer | -2,147,483,648 | 2,147,483,647 |
| uint32 | unsigned 32-bit integer | 0 | 4,294,967,295 |
| int64 | 64-bit integer | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |
| uint64 | unsigned 64-bit integer | 0 | 18,446,744,073,709,551,615 |
| float | 소수점 이하 7자리까지 입력할 수 있습니다. | -3.4 × 10^38 | 3.4 × 10^38 |
| bool | `true` 또는 `false` | | |
| json | json 포맷으로 최대 1024 bytes까지 입력할 수 있습니다. | | |
| string | 최대 100 bytes까지 입력할 수 있습니다. | | |
| datetime | 날짜와 시간을 입력할 수 있습니다.| | |

<!-- ![world server 6](/img/docs/guide/world/world-server/remote-config-value.png) -->

### 연결된 데이터 테이블

데이터 테이블을 월드와 연결하여 월드 서버 시작 시 불러올 초기 데이터를 설정할 수 있습니다.

:::caution
데이터 테이블의 변경사항을 월드 서버에 반영하려면 시뮬레이션 서버를 **다시 시작**해야 합니다.
:::

<!-- ![world server 7](/img/docs/guide/world/world-server/connected-data-table.png) -->
<!-- ![world server 8](/img/docs/guide/world/world-server/connected-data-table-name.png) -->
![world server 9](/img/docs/guide/world/world-server/connected-data-table-version.png)


### 설정

- 월드 서버의 **리전**과 **최대 CCU**를 관리할 수 있습니다.
- 월드 서버의 예상 월 요금을 확인할 수 있습니다.

:::info 안내
베타 기간 동안에는 리전(Seoul), 최대 CCU(100)으로 고정되며, **모든 기능이 무료로 제공**됩니다.
:::

![world server 10](/img/docs/guide/world/world-server/setting.png)

### 월드 서버 삭제

- 월드 서버는 시뮬레이션 서버 상태가 `Stopped` 또는 `Error` 상태에서만 삭제 가능합니다.
- 삭제 시 월드 서버의 모든 데이터(빌드 파일, 리모트 컨피그)는 복구할 수 없습니다. 단, 데이터 테이블은 월드와 함께 삭제되지 않으며, '차트' 메뉴에서 확인할 수 있습니다.

![world server 11](/img/docs/guide/world/world-server/delete.png)


