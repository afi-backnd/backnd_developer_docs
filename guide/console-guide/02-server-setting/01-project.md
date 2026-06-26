import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 프로젝트

## 프로젝트 설정
뒤끝 콘솔 **[프로젝트 설정 - 프로젝트]**에서는 뒤끝 콘솔에서 사용할 프로젝트의 정보들을 설정할 수 있습니다.  

<ConsoleLinkButton text="프로젝트 바로가기" menu="settingProject" feature="프로젝트" title="프로젝트" />

<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정_프로젝트.png" />

### 1. 프로젝트 이름
프로젝트 설정 시의 필수 항목이며 언제든 변경 가능합니다.  

### 2. 프로젝트 상태
프로젝트의 상태 값을 설정합니다.  
* 프로젝트 상태가 온라인/오프라인인 경우 뒤끝 사용 시 어떤 영향도 주지 않습니다.  
* 프로젝트 상태가 점검인 경우 SDK에서 서버로 요청을 보낼 시 모든 요청은 실패하게 됩니다.  

> [SDK에서 함수 호출](/sdk-docs/backend/base/sdk-utils/get-project-status) 시 해당 서버 상태를 받아올 수 있습니다.  

## 프로젝트 삭제
페이지 상단의 **삭제 버튼(휴지통 모양)**을 클릭하고, 프로젝트 이름과 관리자 비밀번호를 입력하면 해당 프로젝트를 삭제하실 수 있습니다.  
다음과 같은 경우에는 프로젝트 삭제가 불가능합니다.  

- 프로젝트 삭제 시점에 현재 발생된 요금이 존재할 경우
- 청구서 발행일(매월 5일)일 경우

<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정_프로젝트삭제.png" />

  


<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정_프로젝트삭제2.png" />

## 프로젝트 복제
페이지 상단의 **복제 버튼** 클릭하면, 프로젝트를 복사할 수 있습니다.
복제에는 최대 5분이 소요됩니다. 
![프로젝트 복제 버튼](/img/docs/guide/project/projectcopy-01.png)
![프로젝트 복제 모달](/img/docs/guide/project/projectcopy-02.png)

프로젝트에서 복사가 가능한 항목은 아래와 같습니다.

- 서버 설정(인증정보, 스토어 정보)  
  사이드 메뉴의 인증 정보, 스토어 정보에 입력된 내용을 복사할 수 있습니다.  

  ![프로젝트 서버 설정 복제](/img/docs/guide/project/projectcopy-03.png)
  서버 설정 포함 시 복사되는 메뉴입니다.

* 게임 정보 테이블  
  게임 정보 메뉴의 테이블을 복사하는 기능입니다.  
  스키마 정의 테이블의 경우 컬럼 정보까지, 미정의 테이블은 테이블 명만 복제됩니다.    

  ![프로젝트 게임 정보 복제](/img/docs/guide/project/projectcopy-04.png)
  스키마 정의 테이블 포함 시 컬럼 정보가 복사됩니다.
