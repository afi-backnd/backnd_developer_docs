---
description: "버전 관리"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 버전 관리

버전 관리 기능은 클라이언트 버전과 대조하여 서버 버전이 더 높을 경우 **강제/선택** 값에 따라 업데이트 처리 방법을 제공해 주는 기능입니다.  

- 버전 수치가 아닌 가장 최근에 등록된 버전이 실제 적용됩니다.(플랫폼별)
- 버전 삭제 시 최근 등록일 기준으로 다음 버전이 적용됩니다.(플랫폼별)

<ConsoleLinkButton text="버전 관리 바로가기" menu="baseVersion" feature="버전 관리" title="버전 관리" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/버전관리/뒤끝베이스--버전-관리.png" />

## 버전 등록

페이지 상단의 명령 버튼 중 **버전 등록** 버튼을 클릭하여 새로운 버전을 등록할 수 있습니다.  

- SDK에서 함수 호출 시, 업데이트 방식이 강제일 경우 type이 1로, 선택일 경우 type이 2로 값이 리턴됩니다.  
- 버전의 제목은 영문, 숫자, '.'만 입력이 가능하며 최대 20자까지 작성할 수 있습니다.  
- 버전은 IOS/안드로이드 각각 추가할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/버전관리/뒤끝베이스--버전-등록.png
" /> 

<img src="https://developer.thebackend.io/static/img/newconsole/base/버전관리/뒤끝베이스--버전-등록-모달.png" />

## 버전 삭제

페이지 상단의 명령 버튼 중 **삭제 버튼**을 클릭하여 버전을 삭제하실 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/newconsole/base/버전관리/뒤끝베이스--버전-등록---버전-삭제.png" />
