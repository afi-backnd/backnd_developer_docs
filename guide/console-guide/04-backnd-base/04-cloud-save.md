---
sidebar_label: 클라우드 세이브
position: 3.5
---
import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 클라우드 세이브

유저 데이터를 자유로운 JSON 형식으로 저렴하게 저장하고 관리해 보세요.

- 유저의 게임 설정, 개인 기록, 비실시간 진행 상황 등을 저장할 수 있습니다.
- 실시간으로 저장하고 관리하려면 [데이터베이스](/guide/console-guide/database/introduction/) 또는 [게임 정보](/guide/console-guide/backnd-base/game-information/table) 기능을 사용하세요.

<ConsoleLinkButton text="클라우드 세이브 바로가기" menu="baseCloudSave" feature="클라우드 세이브" title="클라우드 세이브" /><br/><br/>
  

:::warning 클라우드 세이브 이용 시 주의 사항
클라우드 세이브는 **저비용 데이터 저장**에 최적화된 기능입니다.  
이에 따라 다음 기능이 지원되지 않으니, 서비스 용도에 맞춰 도입을 결정해 주시기 바랍니다.

- **데이터 백업 미지원**: 자동 데이터 백업이 제공되지 않습니다.
- **서버 요청 로그 미저장**: 서버 요청에 대한 상세 로그가 저장되지 않습니다. 에러 발생 등의 상황 추적이 필요한 경우, 클라이언트 측에서 직접 로그를 관리해 주셔야 합니다.

데이터 유실 리스크가 있거나 정밀한 추적이 필요한 중요 정보는 **데이터베이스** 또는 **게임 정보** 이용을 권장합니다.
:::

## 컬렉션 생성하기

컬렉션은 유저 데이터를 그룹화하여 관리할 수 있는 기능입니다.

- 컬렉션은 최대 20개까지 생성 가능합니다.

![cloud save 1](/img/docs/guide/base/cloud-save/create.png)

## 컬렉션 목록

컬렉션 목록에서 생성된 컬렉션 이름을 확인할 수 있습니다.

- 컬렉션을 삭제할 경우 컬렉션에 있는 모든 데이터가 삭제되며 다시 복구할 수 없습니다.
- 컬렉션 목록은 알파벳 순서로 정렬됩니다.

![cloud save 2](/img/docs/guide/base/cloud-save/collection-list.png)

## 유저 데이터 생성하기

원하는 컬렉션에서 유저 UUID와 JSON 형식의 데이터를 입력하여 유저 데이터를 생성할 수 있습니다.

- ⚠️ 이미 생성된 유저 UUID로 재생성할 경우 기존 데이터가 새로 입력된 데이터로 덮어씌워집니다. 기존 데이터가 손실될 수 있으니 주의해 주세요.
- 유저데이터는 두 가지 모드로 생성 및 관리할 수 있습니다: 기본 편집 모드, 직접 작성 모드
- 일반적인 응답 시간은 평균 800ms이지만 처리량이 많은 경우, 2초 이상 응답 지연이 발생할 수 있습니다.

### 기본 편집 모드

- 트리형 구조에 따라 JSON 데이터를 입력 및 수정할 수 있습니다.
- 형식 문제, 오입력 등으로 인한 오류 가능성이 낮으므로 기본 편집 모드의 사용을 권장합니다.

![cloud save 3](/img/docs/guide/base/cloud-save/user-data-create1.png)

### 직접 작성 모드

- 자유롭게 데이터를 입력 및 수정할 수 있습니다.
- 잘못된 데이터 형식은 유저 정보에 오류를 발생시킬 수 있으므로 입력 시 유의해 주세요.

![cloud save 4](/img/docs/guide/base/cloud-save/user-data-create2.png)

## 유저 데이터 목록

- 목록에서 생성된 유저 데이터의 유저 UUID와 마지막 수정 시각을 확인할 수 있습니다.
- 삭제된 유저 데이터는 복구가 불가능합니다.
- 목록에서 수정 및 삭제 작업이 가능합니다.
- 유저 데이터 목록은 알파벳 순서로 정렬됩니다.

![cloud save 5](/img/docs/guide/base/cloud-save/user-data-list.png)
