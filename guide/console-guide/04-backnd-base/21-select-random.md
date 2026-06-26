import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 랜덤 조회

뒤끝 콘솔에서 유저/길드 랜덤 조회를 생성, 삭제, 조회할 수 있습니다.  
랜덤 조회 대상 설정은 SDK에서 수행합니다.  

<ConsoleLinkButton text="랜덤 조회 바로가기" menu="baseRandomSearch" feature="랜덤 조회" title="랜덤 조회" />

## 랜덤 조회 목록

랜덤 조회 목록에서 다음 정보를 확인할 수 있습니다.  

- 랜덤 조회 명
- UUID
- 분류 : 랜덤 조회 유형(유저/길드)
- 생성일 : 콘솔에서 랜덤 조회를 생성한 일시
<img src="https://developer.thebackend.io/static/img/newconsole/base/랜덤 조회/list.png" />

## 랜덤 조회 생성

랜덤 조회는 유저/길드 합산 최대 50개까지 생성할 수 있습니다.  

- 분류에서 유저/길드를 선택할 수 있습니다.  
- 랜덤 조회 명은 1-16자 이내의 영문 대소문자, 숫자, 한글, 공백을 사용할 수 있으며, 필수 입력 항목입니다.  
![랜덤 조회 생성](/img/docs/guide/base/select-random/create-pool.png)



랜덤 조회 시 그룹의 구분 여부를 설정할 수 있습니다. (SDK 5.18.9 이상 사용 가능)
- **구분함**: 랜덤 조회 시 해당 유저/길드의 그룹 내에서만 조회됩니다.
- **구분 안 함**: 랜덤 조회 시 전체 유저/길드 대상으로 조회됩니다.


[오픈형 길드](https://docs.backnd.com/guide/console-guide/backnd-base/guild/manipulation/#길드-유형)의 경우에도 랜덤 조회의 설정이 우선 적용됩니다.

| 랜덤 조회 시 그룹 구분 | 길드 유형 | 조회 대상 |
|---|---|---|
| 구분함 | 오픈형/그룹형 | 동일 그룹 유저/길드 |
| 구분 안 함 | 오픈형/그룹형 | 전체 유저/길드 |

## 랜덤 조회 생성 불가

랜덤 조회를 50개 초과하여 생성할 경우, 다음과 같은 에러 모달이 표시됩니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/랜덤 조회/maxLimit5.png" />

## 랜덤 조회 삭제

랜덤 조회를 삭제할 수 있습니다.  

- 대상을 하나 이상 선택해야 삭제 버튼이 활성화됩니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/랜덤 조회/deletePool.png" />

## 랜덤 조회 상세 조회

랜덤 조회에 포함된 대상의 수 및 정보를 확인하고 검색할 수 있습니다.  

- 랜덤 조회에 포함된 전체 대상의 수가 출력됩니다.  
- 유저 랜덤 조회는 회원번호와 inDate, 스코어가 출력됩니다.  
- 유저 랜덤 조회는 회원번호로 대상 검색이 가능합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/랜덤 조회/getUserPool.png" />
- 길드 랜덤 조회는 길드명과 inDate, 스코어가 출력됩니다.  
- 길드 랜덤 조회는 길드명으로 대상 검색이 가능합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/랜덤 조회/getGuildPool.png" />

## 랜덤 조회 상세 삭제/초기화

랜덤 조회에 포함된 대상을 개별/일괄 삭제하거나 전체 초기화할 수 있습니다.  

- 초기화 시 해당 랜덤 조회에 포함된 모든 대상을 삭제합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/랜덤 조회/initPool.png" />
- 대상을 하나 이상 선택해야 삭제 버튼이 활성화됩니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/base/랜덤 조회/deleteUsers.png" />
