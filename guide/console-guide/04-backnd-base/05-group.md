---
sidebar_label: 그룹
position: 4.5
---
import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 그룹

유저들을 그룹으로 나눠서 관리하고, 그룹 안에 있는 유저들끼리 집계되는 리더보드를 만들 수 있습니다.

<ConsoleLinkButton text="그룹 바로가기" menu="baseUserGroup/entry" feature="그룹" title="그룹" />

## 그룹 시작하기

처음 그룹 메뉴 진입 화면에서 [지금 바로 시작하기 →] 버튼을 클릭해 그룹 기능을 사용할 수 있습니다.

![group start](/img/docs/guide/base/group/group-onboarding.png)

## 그룹 목록

- NULL 그룹은 기본으로 존재하는 그룹입니다. 유저를 생성할 때 별도의 그룹 지정이 없으면 자동으로 NULL 그룹으로 배정됩니다.
- 그룹 목록에는 해당 그룹의 그룹 이름, 유저 수, 활성 유저 비율, 생성일 정보가 노출됩니다.
- 활성 유저 비율은 ‘DAU / 전체 유저 수(%)’입니다. 최근 1일의 활성 유저 비율과 그 전날을 비교한 증감률도 함께 보여줍니다.

![group list 1](/img/docs/guide/base/group/group-list.png)

### 그룹 선택

여러 그룹을 선택하여 그룹 통합하고 삭제하기가 가능합니다.

![group list 2](/img/docs/guide/base/group/group-list-select.png)

### 그룹 검색

완전 일치(equals) 검색 방식으로 그룹 이름을 검색할 수 있습니다.

![group list 3](/img/docs/guide/base/group/group-search.png)

## 그룹 생성하기

- 새 그룹 생성하기 버튼을 클릭하여 그룹 이름을 입력하고 생성할 수 있습니다.
- 그룹 이름은 영어 대소문자, 숫자, ‘-’를 사용하여 100자 이하로 입력할 수 있습니다.
- 그룹은 500개까지 생성할 수 있습니다.

![group create](/img/docs/guide/base/group/group-create.png)

## 그룹 상세

그룹을 클릭하면 그룹 상세 페이지가 열립니다. 그룹 정보와 그룹 통계, 그룹에 속한 유저와 길드, 리더보드 목록을 확인할 수 있습니다.

### 그룹 통계

- 그룹 통계를 제공하여 해당 그룹의 지표를 확인할 수 있습니다.
- 통계를 참고하여 해당 그룹을 통합할지 여부를 판단할 수 있습니다.

![group detail 1](/img/docs/guide/base/group/group-detail-statistics.png)

### 유저와 리더보드

**유저**

:::info 안정적인 사용을 위해 그룹 내 유저 수는 최대 50,000명까지를 권장합니다.  
:::

- 해당 그룹에 속한 유저의 숫자와 목록을 확인할 수 있습니다.
- UUID와 닉네임으로 유저를 검색할 수 있습니다. 유저는 완전 일치(equals) 방식으로 검색됩니다.

![group detail 2](/img/docs/guide/base/group/group-detail-user.png)

**길드**

- 해당 그룹에 속한 길드의 숫자와 목록을 확인할 수 있습니다.
- 길드 이름으로 검색할 수 있습니다. 길드는 부분 일치(contains) 방식으로 검색됩니다.

![group detail 3](/img/docs/guide/base/group/group-detail-guild.png)

**리더보드**

- 그룹별로 집계되는 리더보드의 숫자와 목록을 확인할 수 있습니다.
- 리더보드 이름으로 검색할 수 있습니다. 리더보드는 부분 일치(contains) 방식으로 검색됩니다.

![group detail 4](/img/docs/guide/base/group/group-detail-leaderboards.png)

### 다른 그룹으로 내보내기

목록에서 유저/길드를 최대 100명까지 선택하여 다른 그룹으로 내보내기 할 수 있습니다.

![group detail 5](/img/docs/guide/base/group/group-detail-move.png)

![group detail 6](/img/docs/guide/base/group/group-detail-move-modal.png)


- 그룹별 리더보드의 경우 초기화 옵션을 선택할 수 있습니다.

  | 옵션 | 설명 | 초기화 데이터 | 초기화 대상 |
  |---|---|---|---|
  | 1 | 이동한 유저/길드가 대상 그룹의 리더보드로 통합됩니다.<br />점수 및 집계 필드 데이터가 유지되며 추가된 유저/길드를 포함하여 순위가 재정렬됩니다. | - | - |
  | 2 | 이동한 유저/길드만 리더보드 순위, 점수 및 집계 필드 데이터를 초기화합니다.<br />대상 그룹의 기존 데이터는 변경되지 않습니다. | 순위, 점수, 집계 필드 | 이동한 유저/길드 |
  | 3 | 이동한 유저/길드만 리더보드 순위, 점수를 초기화합니다. (집계 필드 데이터는 유지)<br />대상 그룹의 기존 데이터는 변경되지 않습니다. | 순위, 점수 | 이동한 유저/길드 |

<br />

- [길드 유형(그룹형/오픈형)](https://docs.backnd.com/guide/console-guide/backnd-base/guild/manipulation/#길드-유형)에 따라 유저 및 길드의 이동 결과가 달라집니다.

  <table>
    <thead>
      <tr>
        <th style={{width: '120px'}}></th>
        <th>그룹형 길드</th>
        <th>오픈형 길드</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><strong>길드 이동 시</strong></td>
        <td>길드와 소속 유저가 모두 이동합니다.</td>
        <td>길드만 이동하며, 유저는 기존 그룹에 남습니다.</td>
      </tr>
      <tr>
        <td><strong>유저 이동 시</strong></td>
        <td>유저의 길드 소속이 해제됩니다. <br />길드마스터는 다른 그룹으로 이동할 수 없습니다.</td>
        <td>유저의 길드 소속이 유지됩니다. <br />길드마스터도 다른 그룹으로 이동할 수 있습니다.</td>
      </tr>
    </tbody>
  </table>

<br />

### 그룹 통합하고 삭제하기

- 그룹 내 유저가 존재할 경우 다른 그룹으로 이전하여 통합하고 그룹을 삭제할 수 있습니다.
- 삭제 후 30일 동안 동일한 이름으로 그룹을 생성할 수 없습니다. 삭제된 후 30일이 지나면 생성 가능합니다.
- 기존 그룹에서 수령하지 않은 리더보드 보상은 그룹을 통합한 후에는 수령할 수 없습니다.

![group detail 7](/img/docs/guide/base/group/group-detail-merge.png)


- 그룹별 리더보드의 경우 초기화 옵션을 선택할 수 있습니다.

  | 옵션 | 설명 | 초기화 데이터 | 초기화 대상 |
  |---|---|---|---|
  | 1 | 두 그룹의 리더보드를 통합합니다.<br />통합된 유저/길드 점수에 따라 순위가 재정렬됩니다. | - | - |
  | 2 | 두 그룹의 리더보드를 통합할 때 순위, 점수, 집계 필드 데이터를 초기화합니다. | 순위, 점수, 집계 필드 | 두 그룹의 유저/길드 전체 |
  | 3 | 이동한 유저/길드만 리더보드 순위, 점수 및 집계 필드 데이터를 초기화합니다.<br />대상 그룹의 기존 데이터는 변경되지 않습니다. | 순위, 점수, 집계 필드 | 이동한 유저/길드 |
  | 4 | 이동한 유저/길드만 리더보드 순위, 점수를 초기화합니다. (집계 필드 데이터는 유지)<br />대상 그룹의 기존 데이터는 변경되지 않습니다. | 순위, 점수 | 이동한 유저/길드 |

- 그룹별 리더보드 설정은 통합 대상 그룹의 유저/길드만 진행되며, 통합 대상 외 다른 그룹에는 영향이 없습니다.


