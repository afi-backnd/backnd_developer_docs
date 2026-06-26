import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 유저 정보 찾기

**유저 정보 찾기**를 사용해 커스텀 계정의 아이디나 비밀번호를 해당 이메일로 전송하는 데 필요한 템플릿을 작성할 수 있습니다.  
클라이언트에서의 기능은 [개발자 문서-커스텀 유저 정보 찾기](/sdk-docs/backend/base/user/custom/password)를 참고해 주세요

- 정보 찾기용 유저 이메일이 등록되어 있어야 합니다
- 해킹 시도, 악용 방지 등의 이유로 한 유저당 하루에 5회까지만 사용할 수 있습니다.(아이디, 비밀번호 통합하여 이메일 5회 발송 제한)
- 동일한 정보 찾기용 이메일에 여러 아이디가 존재한다면 콤마로 구분하여 모두 출력합니다
- 모든 항목은 필수 입력이며 내용에는 각 치환자가 반드시 포함되어야 합니다.  

<ConsoleLinkButton text="유저정보 찾기 바로가기" menu="baseSearchGamer/id" feature="유저정보 찾기" title="유저 정보 찾기" />

## 아이디 찾기

클라이언트에서 아이디 찾기 요청 시 등록된 정보 찾기용 이메일로 아이디 찾기에 입력한 정보를 발송합니다.  

- 이메일 내용에 아이디 치환자 **${userId}**가 반드시 포함되어야 합니다.  
- SDK 버전이 5.11.0 이상 인 경우는 언어를, 5.10.2 이하인 경우는 국가코드 메시지 템플릿을 사용할 수 있습니다.  

![언어별 아이디 찾기](/img/docs/guide/search-user/searchuser-01.png)
    △ 언어별 아이디 찾기

![국가별 아이디 찾기](/img/docs/guide/search-user/searchuser-02.png)
    △ 국가코드별 아이디 찾기
<!-- <img src="https://developer.thebackend.io/static/img/newconsole/base/유저 정보 찾기/뒤끝베이스--유저-정보-찾기---아이디-찾기.png" /> -->

## 비밀번호 초기화

클라이언트에서 비밀번호 초기화 요청 시 임시 비밀번호를 생성해 정보 찾기용 이메일로 **비밀번호 초기화**에 입력한 정보를 발송합니다.  
- 임시 비밀번호는 영문 소문자+숫자 조합의 8자로 랜덤하게 생성합니다.  
- 이메일 내용에 임시 비밀번호 치환자 **${newPassword}** 가 반드시 포함되어야 합니다.  
- SDK 버전이 5.11.0 이상 인 경우는 언어를, 5.10.2 이하인 경우는 국가코드 메시지 템플릿을 사용할 수 있습니다.  

![언어별 비밀번호 찾기](/img/docs/guide/search-user/searchuser-03.png)
    △ 언어별 비밀번호 찾기

![국가별 비밀번호 찾기](/img/docs/guide/search-user/searchuser-04.png)
    △ 국가코드별 비밀번호 찾기
<!-- <img src="https://developer.thebackend.io/static/img/newconsole/base/유저 정보 찾기/뒤끝베이스--유저-정보-찾기---비밀번호-초기화.png" /> -->

## 언어/국가코드 추가

**아이디 찾기**와 **비밀번호 초기화** 항목에 **DEFAULT** 우측에 있는 + 버튼을 눌러 다른 언어/국가코드용 메시지 템플릿을 추가할 수 있습니다.  
SDK 버전이 5.11.0 이상 인 경우는 언어를, 5.10.2 이하인 경우는 국가코드 메시지 템플릿을 사용할 수 있습니다.  

![언어추가](/img/docs/guide/search-user/searchuser-05.png)
    △ 언어별 메시지 템플릿 관리
    
![국가코드추가](/img/docs/guide/search-user/searchuser-06.png)
    △ 국가코드별 메시지 템플릿 관리
<!-- <img src="https://developer.thebackend.io/static/img/newconsole/base/유저 정보 찾기/뒤끝베이스--유저-정보-찾기---국가-추가-버튼.png" /> -->

<!-- <img src="https://developer.thebackend.io/static/img/newconsole/base/유저 정보 찾기/뒤끝베이스--유저-정보-찾기---국가-추가-.png" /> -->
