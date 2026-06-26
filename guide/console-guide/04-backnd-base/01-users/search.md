---
sidebar_label: 유저 정보 조회
sidebar_position: 2
---
import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 유저 관리 - 유저 정보 조회

뒤끝 콘솔의 유저 관리에서는 유저의 정보를 확인할 수 있습니다.

:::info 유저 아이디
- 커스텀 로그인의 경우 유저가 설정한 아이디로 유저 아이디가 설정됩니다.
- 페더레이션 로그인의 경우 이메일이 유저 아이디로 설정됩니다.
  - 이메일이 없을 경우 '-'로 표시됩니다.
- 커스텀 계정을 페더레이션 계정으로 변경한 경우 이메일이 유저 아이디로 설정됩니다.
- 이메일이 없을 경우 커스텀 계정의 아이디가 그대로 유저 아이디로 표시됩니다.
:::

<ConsoleLinkButton text="유저 바로가기" menu="baseGamer/5.11.0/gamer" feature="유저" title="유저 관리 - 유저 정보 조회" />

## 유저 정보 조회(멀티 캐릭터 미사용)

유저 번호를 클릭하여 해당 유저의 정보를 조회할 수 있습니다.  
유저 정보 수정 시, 하단에 있는 **확인** 버튼을 눌러야 수정 사항이 적용됩니다.  
주의. 차단된 유저에 대한 **차단 해제** 기능은 해제 버튼을 누르면 별도로 적용됩니다.

![userinfo enquiry 1](/img/docs/guide/base/user-management/userinfo_enquiry.png)

- 유저 번호
- 유저 아이디 : 페이스북, 구글 등으로 가입 시 수집된 유저 아이디 출력(custom 로그인의 경우, 입력한 아이디 출력)
- 닉네임 : 닉네임이 없을 경우 '-'로 출력.
- 그룹 : 그룹 기능을 사용하면 해당 유저의 소속된 그룹 정보 출력, 그룹 클릭 시 그룹 상세 페이지로 이동
- 가입일
- 최종 접속일
- 가입유형
- TBC : 결제를 하지 않았을 경우 '-'로 출력. 결제를 했을 경우, 내역 보기 버튼이 생성되며 클릭 시 **게임 캐시 관리-사용내역** 메뉴로 이동
- 접속 OS
- 접속 기기
- 길드 : 길드 정보가 있을 시 길드 명 출력, 없을 시 '-'로 출력
- 국가 : 국가가 설정되어 있을 경우 해당 국가 국기 출력. 없을 시 '-'로 출력
- 친구 : **친구 목록** 버튼 클릭 시 유저의 친구 목록 출력
- 우편 : **우편 목록** 버튼 클릭 시 유저의 우편 내역 출력
- 디바이스 정보 : 유저가 접속한 이력이 있는 디바이스 목록 출력
- 기타 정보 : 뒤끝 회원가입 함수 호출 시에 사용된 인자 값 **String ect** 출력
- 누적 결제액 : 유저의 누적 결제액 출력(SDK 5.12.2 이후 버전을 통해 저장된 경우에만 표기)
- 유저 차단 기간 : 기간 출력 및 유저 차단 / 해제 가능
- 유저 차단 사유
- 탈퇴 여부
- 탈퇴 신청일시(탈퇴 예정일시)
- 탈퇴 사유
- 메모 : 관리 용도로 자유롭게 입력 가능

### 유저 그룹 정보
그룹 기능을 사용하면 해당 유저의 소속된 그룹 정보를 볼 수 있습니다.

![user group info 1](/img/docs/guide/base/user-management/user_group_info.png)

그룹 클릭 시 해당 그룹의 상세 페이지로 이동합니다.

![user group info 2](/img/docs/guide/base/user-management/user_group_detail.png)

그룹 상세 페이지에서 유저를 다른 그룹으로 이동시킬 수 있습니다.

![user group info 3](/img/docs/guide/base/user-management/user_group_move.png)

### 유저 닉네임 변경

해당 유저 정보의 **닉네임**란에 **변경** 버튼을 눌러 유저의 닉네임을 변경할 수 있습니다.

- 닉네임이 없는 유저의 경우 입력한 내용으로 닉네임이 생성됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-닉네임-변경.png" />

### 유저 친구 목록 조회

해당 게임 유저 정보의 **친구** 항목의 **친구 목록** 버튼을 눌러 해당 유저의 친구 목록을 조회할 수 있습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--친구-목록.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--친구-목록-모달.png" />

### 유저 우편 목록 조회

**우편** 항목의 **우편 목록** 버튼을 눌러 해당 유저가 수신한 우편 목록을 조회할 수 있습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--우편-목록.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--우편-목록-모달.png" />

### 디바이스 정보

**디바이스 정보** 항목의 **디바이스 목록** 버튼을 눌러 해당 유저가 게임 접속 시 사용한 디바이스 내역을 볼 수 있습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--디바이스-목록.png" />

접근을 차단할 디바이스를 선택 후 **접근 차단 등록** 버튼을 클릭하면 **유저 접근 관리** 메뉴의 접근 차단 화면으로 이동합니다.  
해당 화면에서 차단을 등록할 수 있습니다.  
**콘솔 가이드 - 뒤끝베이스 - 유저 접근 관리 참조**

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--디바이스-목록--차단-등록.png" />

### 유저 차단

유저를 차단하여 로그인이 불가능하도록 처리합니다. "차단" 버튼을 누르면 **유저 접근 관리** 메뉴로 이동하여 해당 유저를 차단할 수 있습니다.

**콘솔 가이드 - 뒤끝베이스 - 유저 접근 관리 참조**

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--차단.png" />

### 유저 차단 해제

차단 상태의 유저 정보에서 **유저 차단 기간** 항목의 **해제** 버튼을 클릭할 경우 해당 유저의 차단이 취소됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--차단해제.png" />

### 유저 탈퇴

해당 게임 유저 정보의 **탈퇴 여부** 항목을 **탈퇴**로 변경 후 **확인** 버튼을 클릭할 시, 탈퇴 신청일로부터 일주일 뒤 탈퇴가 이루어집니다.  
또한 **탈퇴 여부** 항목 우측에 **즉시 탈퇴**를 체크하실 경우, **확인** 버튼을 클릭한 후 곧바로 탈퇴가 이루어집니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-탈퇴.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-탈퇴2.png" />

### 유저 탈퇴 취소

해당 게임 유저 정보의 **탈퇴 여부** 항목을 **정상**으로 변경 후 **확인** 버튼을 클릭할 시, 탈퇴 정보가 초기화되며 탈퇴가 취소됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-탈퇴-취소.png" />

## 유저 정보 조회(멀티 캐릭터 사용)

:::caution

- 멀티 캐릭터 기능에 여러 가지 개선이 필요한 상황임을 확인하고, 현재는 멀티 캐릭터 프로젝트 생성을 제한한 상태입니다. 이미 멀티 캐릭터를 사용 중인 프로젝트에서는 계속 멀티 캐릭터 기능을 이용하실 수 있습니다.
- 만일 멀티 캐릭터 기능을 사용하지 않고 일반 프로젝트로 전환을 원하시는 경우, help@backnd.com로 문의해 주세요.
- 보다 편리하고 안정적으로 사용하실 수 있도록 정비 중이오니 너른 양해 부탁드립니다.

:::

유저 번호를 클릭하여 해당 유저의 정보를 조회할 수 있습니다.  
유저 정보 수정 시, 하단에 있는 **확인** 버튼을 눌러야 수정 사항이 적용됩니다.  
주의. 차단된 유저에 대한 **차단 해제** 기능은 해제 버튼을 누르면 별도로 적용됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-정보-조회--멀티캐릭터.png" />

- 유저 번호
- 유저 아이디: 페이스북, 구글 등으로 가입 시 수집된 유저 아이디 출력(custom 로그인의 경우, 입력한 아이디 출력)
- 생성일
- 최종 접속일
- 유형
- 캐릭터
- 접속 OS
- 접속 기기
- 디바이스 정보: 유저가 접속한 이력이 있는 디바이스 목록 출력
- 기타 정보: 뒤끝 회원가입 함수 호출 시에 사용된 인자 값 **String ect** 출력
- 차단 기간 : 기간 출력 및 유저 차단 / 해제 가능
- 차단 사유
- 탈퇴 여부
- 탈퇴 신청일시(탈퇴 예정일시)
- 탈퇴 사유

### 디바이스 정보

**디바이스 정보** 항목의 **디바이스 목록** 버튼을 눌러 해당 유저가 게임 접속 시 사용한 디바이스 내역을 볼 수 있습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--디바이스-목록--멀티캐릭터.png" />

접근을 차단할 디바이스를 선택 후 **접근 차단 등록** 버튼을 클릭하면 **유저 접근 관리** 메뉴의 접근 차단 화면으로 이동합니다.  
해당 화면에서 차단을 등록할 수 있습니다.  
**콘솔 가이드 - 뒤끝베이스 - 유저 접근 관리 참조**

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--디바이스-목록--차단-등록--멀티캐릭터.png" />

## 유저 누적 결제액
- 유저의 누적 결제액을 확인할 수 있습니다.(SDK 5.12.2 이후 버전을 통해 저장된 경우에만 표기)
- 클릭 시 해당 유저의 영수증 검증 내역을 볼 수 있는 페이지로 이동합니다.

![paymenr_amount](/img/docs/guide/base/user-management/cumulative_payment_amount.png)

### 유저 차단

유저를 차단하여 로그인이 불가능하도록 처리합니다. "차단" 버튼을 누르면 **유저 접근 관리** 메뉴로 이동하여 해당 유저를 차단할 수 있습니다.

**콘솔 가이드 - 뒤끝베이스 - 유저 접근 관리 참조**

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--차단--멀티캐릭터.png" />

### 유저 차단 해제

차단 상태의 유저 정보에서 **유저 차단 기간** 항목의 **해제** 버튼을 클릭할 경우 해당 유저의 차단이 취소됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--차단해제--멀티캐릭터.png" />

### 유저 탈퇴

해당 게임 유저 정보의 **탈퇴 여부** 항목을 **탈퇴**로 변경 후 **확인** 버튼을 클릭할 시, 탈퇴 신청일로부터 일주일 뒤 탈퇴가 이루어집니다.  
또한 **탈퇴 여부** 항목 우측에 **즉시 탈퇴**를 체크하실 경우, **확인** 버튼을 클릭한 후 곧바로 탈퇴가 이루어집니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-탈퇴--멀티캐릭터.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-탈퇴2--멀티캐릭터.png" />

### 유저 탈퇴 취소

해당 게임 유저 정보의 **탈퇴 여부** 항목을 **정상**으로 변경 후 **확인** 버튼을 클릭할 시, 탈퇴 정보가 초기화되며 탈퇴가 취소됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-탈퇴-취소--멀티캐릭터.png" />

## 캐릭터 정보 조회(멀티 캐릭터 사용)

캐릭터 번호를 클릭하여 해당 캐릭터의 정보를 조회할 수 있습니다.  
캐릭터 정보 수정 시, 하단에 있는 **확인** 버튼을 눌러야 수정 사항이 적용됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-정보-조회--캐릭터.png" />

- 캐릭터 번호
- 유저
- 닉네임: 닉네임이 없을 경우 '-'로 출력.
- 생성일
- 최종 접속일
- 유형
- TBC : 결제를 하지 않았을 경우 '-'로 출력. 결제를 했을 경우, 내역 보기 버튼이 생성되며 클릭 시 **게임 캐시 관리-사용내역** 메뉴로 이동
- 접속 OS
- 접속 기기
- 길드: 길드 정보가 있을 시 길드 명 출력, 없을 시 '-'로 출력
- 국가: 국가가 설정되어 있을 경우 해당 국가 국기 출력. 없을 시 '-'로 출력
- 친구: **친구 목록** 버튼 클릭 시 유저의 친구 목록 출력
- 우편: **우편 목록** 버튼 클릭 시 유저의 우편 내역 출력
- 디바이스 정보: 유저가 접속한 이력이 있는 디바이스 목록 출력
- 기타 정보: 뒤끝 회원가입 함수 호출 시에 사용된 인자 값 **String ect** 출력
- 차단 기간: 기간 출력 및 유저 차단 / 해제 가능
- 차단 사유
- 탈퇴 여부
- 탈퇴 신청일시(탈퇴 예정일시)
- 탈퇴 사유
- 메모: 관리 용도로 자유롭게 입력 가능

### 캐릭터 닉네임 변경

해당 캐릭터 정보의 **닉네임**란에 **변경** 버튼을 눌러 캐릭터의 닉네임을 변경할 수 있습니다.

- 닉네임이 없는 캐릭터의 경우 입력한 내용으로 닉네임이 생성됩니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--유저-닉네임-변경--캐릭터.png" />

### 캐릭터 친구 목록 조회

해당 게임 캐릭터 정보의 **친구** 항목의 **친구 목록** 버튼을 눌러 해당 캐릭터의 친구 목록을 조회할 수 있습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--친구-목록--캐릭터.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--친구-목록-모달--캐릭터.png" />

### 캐릭터 우편 목록 조회

**우편** 항목의 **우편 목록** 버튼을 눌러 해당 캐릭터가 수신한 우편 목록을 조회할 수 있습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--우편-목록--캐릭터.png" />

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--우편-목록-모달--캐릭터.png" />

### 디바이스 정보

**디바이스 정보** 항목의 **디바이스 목록** 버튼을 눌러 해당 캐릭터가 게임 접속 시 사용한 디바이스 내역을 볼 수 있습니다.

<img src="https://developer.thebackend.io/static/img/newconsole/base/유저 관리/뒤끝베이스--유저-관리--디바이스-목록--캐릭터.png" />

## 유저 정보 다운로드

유저 정보를 CSV 파일로 다운로드할 수 있습니다.
다운로드는 마스터 계정(뒤끝에 직접 가입한 계정)으로만 가능하며, 다운로드 시, 비밀번호 확인이 필요합니다.

![userinfo download 1](/img/docs/guide/base/user-management/csv-download-password.png)

### 다운로드 사유 입력 및 다운로드 항목 선택

다운로드 사유를 입력 후 다운로드할 수 있으며, 선택 다운로드 항목을 추가로 선택할 수 있습니다.
작성된 다운로드 사유는 다운로드 내역 조회에서 확인할 수 있습니다.

![userinfo download 2](/img/docs/guide/base/user-management/csv-download-settings.png)

- 가입일은 최대 3개월까지 설정할 수 있습니다.
- 선택 다운로드 항목을 적게 선택할 수록 다운로드가 빨리 진행됩니다.
- 한번에 최대 6만행 까지 다운로드 가능하며, 이를 초과하느 경우는 여러회에 나눠서 다운로드 됩니다.

### 다운로드 내역 조회

다운로드 내역 조회는 마스터 계정(뒤끝에 직접 가입한 계정)으로만 가능하며, 다운로드 시, 비밀번호 확인이 필요합니다.
다운로드 내역에는 다운로드 일시, 설정한 가입일, 사유, 추가다운로드 항목가 표기됩니다.

![userinfo download 3](/img/docs/guide/base/user-management/csv-download-history.png)

- 여러 파일을 다운로드 하다 취소한 경우에도 다운로드 내역에는 기록됩니다.
