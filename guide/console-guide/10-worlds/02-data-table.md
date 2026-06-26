---
title: 데이터 테이블
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 데이터 테이블

월드 서버 시작 시 필요한 초기 데이터를 효율적으로 관리하고 업데이트하세요.

- 데이터를 쉽게 관리하고 빠르게 업데이트할 수 있습니다.
- 몬스터 정보, 아이템 정보, 맵 정보, 퀘스트 정보 등 다양한 데이터를 테이블 형태로 저장 및 관리할 수 있습니다.

<ConsoleLinkButton text="데이터 테이블 바로가기" menu="worldDataTable" feature="데이터 테이블" title="데이터 테이블" />
<br/>

## 데이터 테이블 생성하기
- 데이터 테이블은 이름을 입력하고 CSV 파일을 업로드하여 생성할 수 있습니다.
- 데이터 테이블은 차트 개수와 합산하여 최대 200개까지 생성할 수 있습니다.
- 데이터 테이블을 생성하면 최초 버전이 함께 생성되며, 데이터 테이블의 버전은 ‘차트 파일’ 개수와 합산하여 최대 600개까지 저장할 수 있습니다.

![data table 1](/img/docs/guide/world/data-table/create.png)
    

### CSV 파일 업로드
- [예시 파일](/files/DataTableExample.csv)을 다운로드 받아 활용해보세요. 이 예시 파일은 각 행이 하나의 몬스터 정보를 담고 있습니다.
![example csv](/img/docs/guide/world/data-table/data-table-csv-example.png)
- `id`는 뒤끝 서버에서 사용하는 예악어이기 때문에, 열(column) 이름으로 사용할 수 없습니다.
- 열(column) 이름은 영문 대소문자, 숫자, 그리고 '_'를 사용할 수 있으며, 숫자로 시작할 수 없습니다.
- .csv 뿐만 아니라 .xls, xlsx 파일도 업로드할 수 있으나 단일 시트(sheet)로 구성되어야 합니다.
- 파일명에 역슬래시'\'를 사용할 수 없습니다.

## 데이터 테이블 목록

- 목록에서 생성된 테이블의 이름, 연결된 월드 서버, 최신 테이블 버전 정보를 확인할 수 있습니다.

![data table 2](/img/docs/guide/world/data-table/list.png)

## 데이터 테이블 상세

데이터 테이블 상세 페이지에서는 데이터를 직접 수정할 수 있습니다.

### 데이터 수정하기

- 데이터를 직접 클릭하여 즉시 수정할 수 있습니다.
- **열(column) 입력 규칙:**
    - 영문 대소문자, 숫자, 그리고 '_'만 사용할 수 있습니다.
    - 공백은 허용되지 않으며, 숫자로 시작할 수 없습니다.
    - "id"는 예약된 키로 사용할 수 없습니다.

![data table 4](/img/docs/guide/world/data-table/edit.png)

### CSV 업로드

- 새로운 CSV 파일을 업로드하여 기존 데이터를 덮어씌울 수 있습니다.
- 파일 업로드 시 기존 데이터는 새 데이터로 교체됩니다.

![data table 5](/img/docs/guide/world/data-table/csv-upload.png)

### 변경사항 저장하기

- 데이터를 수정한 후 저장하면 새로운 버전으로 저장됩니다.
- 월드 서버에 연결된 데이터 테이블을 수정 후 저장하면 **최신 버전**을 참조하도록 업데이트할 수 있습니다.

:::caution
데이터 테이블의 변경사항을 월드 서버에 반영하려면 시뮬레이션 서버를 **다시 시작**해야 합니다.
:::

![data table 6](/img/docs/guide/world/data-table/save.png)
