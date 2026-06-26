import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 공지사항(구버전)

:::warning 구버전 안내
해당 페이지는 구버전 공지사항 기능에 대한 문서입니다.  
최신 버전 문서는 **[공지사항](/guide/console-guide/backnd-base/notice/notice)**을 참고해 주세요.
:::

**공지사항**을 등록/수정할 수 있으며 임시 공지를 관리할 수 있습니다.  
SDK에서의 함수 호출은 [개발자 문서-공지사항 리스트 조회](/sdk-docs/backend/base/operation/notice/get-list)를 참고해 주세요.  

<ConsoleLinkButton text="공지 바로가기" menu="baseNotice" feature="공지" title="공지사항" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항.png" />

## 공지사항 등록

**공지사항 등록** 버튼을 눌러 새로운 공지사항을 등록할 수 있습니다.   

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항-공지사항등록버튼.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항-공지사항등록-모달.png" />

### 작성자

- 글 작성 시 로그인 되어있는 계정의 닉네임으로 출력됩니다.  

### 게시 일시

- 즉시 게시 : 등록 즉시 게시됩니다.  
- 예약 게시 : 30분 단위로 예약 설정이 가능하며 현재 시간 이전으로는 설정이 불가합니다.  

### 국가 추가

공지사항을 국가별로 다른 내용(다른 언어)을 지정하여 노출시킬 수 있습니다.  
- 공지사항 입력 칸 상단의 **+버튼**을 누르시면 해당 국가 코드의 유저에게 노출시킬 내용 입력 칸이 생성됩니다.  
- 국가 선택에 **국가 코드는 현재 유저들에게 설정되어 있는 국가를 기준으로 표시**됩니다.  
    - 공지사항 내용을 다르게 노출시키고 싶은 국가 코드를 지닌 유저가 생성되어 있어야 해당 국가 코드가 노출됩니다.  
- 국가 지정은 최대 5개까지 할 수 있습니다.  
- 국가 코드를 지니지 않은 유저나 공지사항에 국가 설정이 지정되지 않은 유저에게는 **기본** 탭의 내용이 노출됩니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항-공지사항등록-모달---국가추가.png" />

### 제목

- 필수 항목이며 최대 60자까지 입력할 수 있습니다.  

### 내용

- 필수 항목이며 최대 4,000자까지 입력할 수 있습니다.  

### 이미지 첨부

- 공지사항 출력 시, 내용보다 위에 삽입되며 한 장만 첨부가 가능합니다.  
- 최대 크기는 1MB이며 JPG, PNG만 지원합니다.  

### 버튼 및 URL 생성

- 공지사항 출력 시, 내용 아래에 입력한 버튼 명으로 버튼이 생성되며 클릭 시 입력한 URL로 이동합니다.  
- 버튼 이름 항목은 30자 까지, URL 생성 항목은 200자 까지 입력할 수 있습니다.  

### 공개 여부

- 비공개 설정 시 노출되지 않습니다.  

## 공지사항 수정

수정 버튼을 클릭하여 등록된 공지사항을 수정할 수 있습니다.  
- 게시 일시는 과거로 변경이 불가합니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항-공지사항-편집.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항-공지사항-편집-모달.png" />

## 공지사항 삭제

페이지 상단의 명령 버튼 중  **삭제** 버튼을 눌러 공지사항을 삭제할 수 있습니다. 

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항-공지사항-삭제.png" />

## 임시 공지 관리

임시 공지는 일반 공지사항과 별개의 공지사항이며 1건만 가능합니다.  
SDK에서의 함수 호출은 [개발자 문서-임시 공지 조회](/sdk-docs/backend/base/operation/notice/get-temporary-one)를 참고해 주세요.  
임시 공지의 사용목적은 다음과 같습니다.  
- 로그인하지 않은 회원에게 노출하고 싶은 정보가 있는 경우
- 강제 업데이트 문구 혹은 개발자가 임의로 서버를 중단 시킬 시 띄울 문구를 노출하고자 하는 경우
- 공지 내용 항목은 4000자 까지 입력할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항-임시공지-관리.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/공지사항/뒤끝베이스-공지사항-임시공지-관리-모달.png" />
