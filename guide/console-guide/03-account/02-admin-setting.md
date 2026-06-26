import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 관리자 계정 관리

## 최고 관리자와 일반 관리자

- 최고 관리자 : 뒤끝 회원가입한 이메일 형식의 계정. GNB(우측 상단)의 드롭다운 메뉴와 알림 기능은 최고 관리자 계정에만 제공됩니다.  
- 관리자 : 최고 관리자가 생성한 관리자 계정. 최고 관리자가 선택한 프로젝트에 역할별로 접근 가능합니다.  

## 관리자 계정 관리

뒤끝 상단(GNB)의 `드롭다운 메뉴 → 관리자 계정 관리` 메뉴에서 관리자 계정을 설정할 수 있습니다.  

<ConsoleLinkButton text="관리자 계정 관리 바로가기" href="https://console.thebackend.io/account/admin/manage-account" feature="관리자 계정 관리" title="관리자 계정 관리" />

![이미지](/img/docs/guide/admin-account/admin-account-01-entry.png)

## 관리자 로그인 URL 설정

먼저 관리자 계정 로그인 URL을 생성해야 합니다.  
![이미지](/img/docs/guide/admin-account/admin-account-03-URL_생성_중.png)

URL은 3-12자 이내의 영문, 숫자로만 생성할 수 있고, 숫자로 시작할 수 없습니다.  
관리자는 해당 URL을 통해서만 로그인할 수 있습니다.  
![이미지](/img/docs/guide/admin-account/admin-account-02-계정목록.png)

## 관리자 계정 생성/수정

- 관리자 계정은 최대 25개까지 생성 가능합니다.  
- 생성/수정 시 관리자가 관리할 프로젝트를 설정할 수 있습니다.  
- 관리자에게 역할 탭에서 생성한 역할을 부여합니다.  
  (최초에는 기본 역할 5개가 제공됩니다.)
- 관리자는 부여된 역할에 포함된 기능만 사용할 수 있습니다.  
(관리자 계정으로 콘솔 로그인 시 역할에 포함되는 메뉴만 출력됩니다.)
- 요금 정보 열람 권한은 역할에 관계없이 별도 부여가 가능하며, 권한이 부여된 관리 프로젝트 전체의 요금을 열람할 수 있습니다.  
(관리 권한이 부여된 프로젝트 중 일부만 요금 열람 권한을 부여할수는 없습니다.)

![관리자 계정 생성 1/2](/img/docs/guide/admin-account/admin-account-04-계정_생성_정보입력.png)
![관리자 계정 생성 2/2](/img/docs/guide/admin-account/admin-account-04-계정_생성_정보입력_2.png)

## 관리자 로그인

관리자 계정은 최고 관리자가 설정한 전용 로그인 URL을 통해서만 로그인이 가능합니다.  
![관리자 로그인](/img/docs/guide/admin-account/admin-account-13-관리자_로그인.png)

공지사항 및 이벤트 등의 작성 시 로그인 한 관리자의 닉네임이 작성자로 등록됩니다.  

## 역할 생성/수정

- **역할이란?**  
  기능별 권한을 커스텀 하여 역할을 생성하고, 해당 역할을 계정에 부여할 수 있다.
- 기본 역할 5개를 제공합니다.  
- 역할은 15개까지 생성 가능합니다.  
- 관리자가 접근 가능한 뒤끝 콘솔 메뉴를 역할별로 정의할 수 있습니다.  
- 생성한 역할은 관리자 계정을 생성/수정할 때 적용할 수 있습니다.  


![관리자 역할 수정](/img/docs/guide/admin-account/admin-account-09-역할_생성_2.png)

### 관리자 기본 역할
기본 역할 5개가 제공됩니다.  

![관리자 기본 역할](/img/docs/guide/admin-account/admin-account-12-역할_기본.png)

### 역할 삭제
역할 삭제 시 일시적으로 관리자의 역할이 초기화될 수 있습니다.  
`역할 삭제/수정 후에는 관리자의 뒤끝 콘솔 웹페이지를 새로고침 후 이용해 주세요.`
![역할 삭제 안내](/img/docs/guide/admin-account/admin-account-11-역할_삭제.png)
