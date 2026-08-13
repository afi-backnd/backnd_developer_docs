---
sidebar_label: "뒤끝매치 예제게임"
description: "TheBackendMatch"
---

# TheBackendMatch

TheBackendMatch는 뒤끝매치를 활용하여 개발한 간단한 슈팅게임입니다.  
해당 예제는 **Unity 2020.1.3f1**과 **Backend-5.9.0-dotnet4**버전을 기준으로 개발되었습니다.  

## 라이센스

TheBackendMatch는 [BSD-2-Clause](https://opensource.org/licenses/BSD-2-Clause) 라이센스를 따릅니다.  

> 소스코드의 수정, 사용, 상업적인 이용이 가능합니다.  
> 소스코드 이용에 따른 책임, 보증은 하지 않습니다.  

해당 유니티 패키지 파일에는 게임 실행을 위한 유니티 무료 에셋인 소스코드와 3D 모델 리소스가 일부 포함되어 있습니다.  

- 모델 혹은 소스코드를 개인 프로젝트에서 사용하고자 할 경우 해당 링크를 참고해 주세요.  
- 3D 모델 리소스에는 아래 유니티 무료 에셋이 포함되어 있습니다.  

  > https://assetstore.unity.com/packages/2d/textures-materials/sky/fantasy-skybox-free-18353
  > https://assetstore.unity.com/packages/3d/environments/landscapes/nature-pack-extended-66146
  > https://assetstore.unity.com/packages/3d/characters/meshtint-free-boximon-fiery-mega-toon-series-153958
  > https://assetstore.unity.com/packages/vfx/particles/spells/48-particle-effect-pack-13998
  > https://assetstore.unity.com/packages/vfx/particles/effect-textures-and-prefabs-109031
  > https://assetstore.unity.com/packages/3d/environments/meshtint-free-tile-map-mega-toon-series-153619

- 소스코드에는 아래 유니티 무료 에셋이 포함되어 있습니다.  
  > https://assetstore.unity.com/packages/tools/dispatcher-41637

## 마켓 링크

- [Google Play Store](https://play.google.com/store/apps/details?id=io.thebackend.backendMatch)
- [Apple App Store](https://apps.apple.com/us/app/thebackendmatch/id1497264852)

## 유니티 패키지 파일

- [BackendMatch.unitypackage](https://developer.thebackend.io/sdk/script/tutorial/5.9.0/BackendMatch.unitypackage)

## 스크린샷

> ![image](https://developer.thebackend.io/static/img/console/matchMake/tutorial.png)

## 포함된 기능

TheBackendMatch는 아래의 뒤끝베이스와 뒤끝챗 기능들을 포함하고 있습니다.  
사용된 뒤끝 기능은 뒤끝의 모든 기능이 아닌, 뒤끝 기능의 일부이며 추후 TheBackendMatch 예제 게임에 추가될 수 있습니다.  

### 뒤끝 베이스 기능

| 기능           | 설명                                                     |
| -------------- | -------------------------------------------------------- |
| 커스텀 계정    | 커스텀 회원가입 및 로그인                                |
| GPGS 로그인    | GPGS 로그인 토큰을 이용한 페더레이션 회원가입 및 로그인  |
| Apple로 로그인 | Apple 로그인 토큰을 이용한 페더레이션 회원가입 및 로그인 |
| 토큰 로그인    | 뒤끝 액세스 토큰을 이용한 토큰 로그인                    |
| 닉네임         | 닉네임 생성                                              |
| 친구           | 친구 신청, 수락, 친구 목록 조회                          |
| SendQueue      | SendQueue를 이용하여 데이터 송신                         |

### 뒤끝 매치 기능

| 기능                          | 설명                           |
| ----------------------------- | ------------------------------ |
| 매치 서버 접속 / 접속 종료    | 매치 서버 접속 및 접속 종료    |
| 인 게임 서버 접속 / 접속 종료 | 인 게임 서버 접속 및 접속 종료 |
| 바이너리 메시지 송수신        | 바이너리 메시지 송수신         |
| 매치 기록 열람                | 매치 기록 열람                 |

## 예제 게임 실행 방법

### 1. 상단 유니티 패키지를 다운로드합니다.  

### 2. 유니티 빈 프로젝트에 해당 패키지를 적용합니다.  

### 3. 뒤끝 콘솔에서 새 프로젝트를 생성합니다.  

> - 해당 프로젝트에서 Client App ID와 Signature Key를 발급받습니다. [해당 문서](/sdk-docs/backend/base/start-up)를 참고하시면 됩니다.  

- 뒤끝 SDK의 경우 이미 소스코드에 포함되어 있습니다. 별도로 설치하지 않으셔도 됩니다.  
- 콘솔의 좌측 메뉴에 **뒤끝매치**를 선택해 뒤끝매치를 활성화합니다.  

### 4. 3번 항목에서 발급받은 Client App ID와 Signature Key를 The Backend > Edit Settings에 입력합니다.  

### 5. 캐릭터에 마테리얼이 적용되어 있지 않을 경우

Prefab이나 모델들에 마테리얼이 적용되지 않고 마젠타색으로 나올 경우, 유니티 상단에 `Window - Package Manager`에서 **Univeral RP(Render Pipeline)**를 인스톨합니다.  

> ![image](https://developer.thebackend.io/static/img/unity/matchMake/tutorial/1.png)

이후, Player Settings에 Graphics 항목에 Scriptable Render Pipeline Settings에 `Asset > LWRP`에 있는 **UniversalRednerPipelineAsset.asset**을 드래그하여 적용시킵니다.  

> ![image](https://developer.thebackend.io/static/img/unity/matchMake/tutorial/2.png)

## 데드레커닝 기법 적용

- 플레이어 이동/회전

  - 클라이언트는 이동 키 패킷을 송신하면 호스트에서는 이동 방향과 클라이언트의 현재 위치를 서버로 전송하여 해당 정보를 브로드캐스팅 시킵니다.  
  - 이 정보를 바탕으로 클라이언트에서는 해당 위치로 클라이언트를 이동시키고, 다음 위치 패킷이 올 때까지 이동방향으로 이동시킵니다.  

- 총알 발사

  - 클라이언트가 총알 발사 키 패킷을 송신하면 호스트에서는 총알 생성 위치와 총알 이동방향을 서버로 전송하여 해당 정보를 브로드캐스팅 시킵니다.  
  - 이 정보를 바탕으로 클라이언트에서는 총알을 생성하고 해당 이동방향으로 이동시킵니다.  

- 총알 피격
  - 호스트에서 플레이어와 총알이 피격되었을 경우 플레이어가 데미지를 입었다는 패킷을 서버로 전송하여 해당 정보를 브로드캐스팅 시킵니다.  
  - 클라이언트에서는 서버에서 송신한 데미지 패킷을 게임에 적용하여 데미지 처리를 수행합니다.  
  - 벽과 총알이 피격한 사실은 동기화를 시켜주지 않습니다. 각 클라이언트에서 자체적으로 처리합니다.  
  - 호스트가 아닌 클라이언트에서 플레이어와 총알이 충돌한 경우에도 무시합니다.(데미지 처리를 하지 않습니다.)
