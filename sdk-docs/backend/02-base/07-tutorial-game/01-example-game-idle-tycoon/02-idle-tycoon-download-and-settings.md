---
sidebar_label: 다운로드 및 게임 설정
sidebar_position: 2
---

# 프로젝트 다운로드 및 게임 설정

**Food Truck Venture** 프로젝트를 정상적으로 실행하려면 유니티와 뒤끝 콘솔에서 필수적으로 다음과 같은 초기 설정 작업을 진행 해야합니다.  
아래는 프로젝트를 정상적으로 실행하기 위한 필수 작업 항목입니다.

- 유니티 - 씬, 빌드 플랫폼 설정
- 유니티 - 뒤끝 콘솔에서 생성된 clientAppId, signatureKey 값 설정
- 유니티 - 스크립터블 오브젝트 설정
- 뒤끝 콘솔 - 게임 정보 테이블 생성
- 뒤끝 콘솔 - 리더보드 세팅 
- 뒤끝 콘솔 - 차트/확률 생성 및 적용

다음은 게임 실행 이후 추가적으로 설정 할 수 있는 항목입니다.  

- 뒤끝 콘솔 - 공지 사항
- 뒤끝 콘솔 - 우편  
- 인앱결제, 영수증 검증

## 프로젝트 다운로드 및 유니티 설정

### 1. GitHub에서 Food Truck Venture프로젝트를 다운로드합니다.  

- 깃허브 링크 : [git repository Food Truck Venture](https://github.com/afi-backnd/base-idle-tycoon-sample)
  - 해당 예제는 **Unity 2022.3.56f1과 Backend-5.17.1 버전**을 기준으로 개발되었습니다.  
  - 구글 로그인 기능은 **뒤끝 구글 로그인 2.0 버전**을 사용합니다.

### 2. 유니티로 Food Truck Venture를 프로젝트를 열어 실행합니다.

- 다운 받은 프로젝트 폴더를 선택하여 유니티로 열어 실행합니다.

### 3. 뒤끝 콘솔에서 새 프로젝트를 생성합니다.  

- [뒤끝 콘솔](https://console.thebackend.io)에 접속하여 새 프로젝트를 생성합니다.  
- 프로젝트를 생성한 뒤 Client App ID와 Signature Key를 확인 합니다. 자세한 방법은 [해당 문서](/sdk-docs/backend/base/start-up)를 참고하시면 됩니다.  
- 뒤끝 SDK의 경우 이미 프로젝트에 적용되어 있습니다. 별도로 설치하지 않으셔도 됩니다.  

### 4. 3번 항목에서 발급받은 Client App ID와 Signature Key를 TheBackendSettings에 입력합니다.  

- 유니티 상단 메뉴에서 **The Backend > Edit Settings**를 클릭하세요.
- Inspector창에서 **Client App ID와 Signature Key**를 붙여넣습니다.  
![image](/img/docs/guide/base/example-games/idle-tycoon/skdSetting.png)

### 5. 유니티 상단 File 탭 클릭 > Build Settings > Scenes in Build에 씬이 등록 되어 있는지 확인 합니다.    

씬은 Assets > Scenes 폴더에서 확인할 수 있습니다.  
씬은 아래의 순서로 등록 되어 있어야 합니다.  

1. StartScene
2. GamePlay


![image](/img/docs/guide/base/example-games/idle-tycoon/sceneSetting.png)


### 6. Build Settings에서 Platform이 Android로 설정되어 있는지 확인합니다.  

Android로 설정되어 있지 않다면 Android를 선택하고 **Switch Platform** 버튼을 눌러 변경합니다.  

개발 환경에 맞게 iOS로 선택하고 진행하셔도 무방합니다.

![image](/img/docs/guide/base/example-games/idle-tycoon/flatformSetting.png)

---

