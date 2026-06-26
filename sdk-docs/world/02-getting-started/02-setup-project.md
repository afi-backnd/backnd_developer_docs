---
sidebar_label: 프로젝트 설정
---

# 프로젝트 설정

## 월드 프로젝트 활성화하기

### 1. 콘솔에 접속하기
먼저 관리자 콘솔에 접속합니다.
![콘솔 접속](/img/docs/guide/world/sdk/setup-project/01.png)

### 2. 설정 메뉴로 이동하기
좌측 메뉴에서 설정을 선택합니다.
![설정 메뉴](/img/docs/guide/world/sdk/setup-project/02.png)

### 3. 월드 기능 활성화하기
설정 페이지에서 "월드 사용하기"를 클릭하여 월드 기능을 활성화합니다.
![월드 기능 활성화](/img/docs/guide/world/sdk/setup-project/03.png)

## 월드 서버 생성하기

### 1. 서버 생성 시작하기
설정 페이지에서 "월드 서버 생성하기" 버튼을 클릭합니다.
![서버 생성 버튼](/img/docs/guide/world/sdk/setup-project/04.png)

### 2. 서버 이름 입력하기
새로운 월드 서버의 이름을 입력합니다.
![서버 이름 설정](/img/docs/guide/world/sdk/setup-project/05.png)

### 3. 서버 생성 완료하기
입력을 마친 후 "생성하기" 버튼을 클릭하여 서버 생성을 완료합니다.
![서버 생성 완료](/img/docs/guide/world/sdk/setup-project/06.png)

## SDK 설치하기

:::note
SDK 설치에 대한 자세한 내용은 [SDK 설치](install) 문서를 참고해 주세요.
:::

## SDK 초기 설정하기

### 1. 콘솔에 접속하기
관리자 콘솔에 접속합니다.
![콘솔 접속](/img/docs/guide/world/sdk/setup-project/01.png)

### 2. 설정 메뉴로 이동하기
좌측 메뉴에서 설정을 선택합니다.
![설정 메뉴](/img/docs/guide/world/sdk/setup-project/02.png)

### 3. Client App ID 설정하기
1. 설정 페이지에서 "Client App ID"를 복사합니다.
   ![Client App ID 복사](/img/docs/guide/world/sdk/setup-project/07.png)

2. Unity의 NetworkManager 컴포넌트에서 Backend Client App ID 필드에 복사한 값을 붙여넣습니다.
   ![Client App ID 설정](/img/docs/guide/world/sdk/install/12.png)

### 4. Signature Key 설정하기
1. 설정 페이지에서 "Signature Key"를 복사합니다.
   ![Signature Key 복사](/img/docs/guide/world/sdk/setup-project/09.png)

2. NetworkManager의 Backend Signature Key 필드에 복사한 값을 붙여넣습니다.
   ![Signature Key 설정](/img/docs/guide/world/sdk/install/13.png)

### 5. 월드 서버 UUID 설정하기
1. 생성된 월드 서버의 "UUID"를 복사합니다.
   ![UUID 위치](/img/docs/guide/world/sdk/setup-project/11.png)
   ![UUID 복사](/img/docs/guide/world/sdk/setup-project/12.png)

2. NetworkManager의 Backend UUID 필드에 복사한 값을 붙여넣습니다.
   ![UUID 설정](/img/docs/guide/world/sdk/install/13.png)
