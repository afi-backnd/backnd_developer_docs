---
sidebar_label: 뒤끝 파일시스템
---

# 뒤끝 파일시스템

뒤끝 SDK 5.0.0 이상을 사용할 때 로컬에 저장되는 데이터들을 다루기 위한 기능입니다.  
뒤끝 파일시스템을 이용하여 아래의 정보들이 로컬에 저장됩니다.  

> SDK 5.0.0 미만 버전에서는 유니티의 PlayerPrefs 기능을 이용하여 데이터를 저장/불러오기 합니다.  

- 액세스 토큰 정보
- 게스트 로그인 계정 정보
- 뒤끝 차트
- 뒤끝 채팅 차단 정보

## 저장 위치 / 저장 파일 명

저장 파일명은 backend.dat입니다.  
로컬에 저장된 데이터는 Application.persistentDataPath/backend.dat에 저장됩니다.  

Application.persistentDataPath는 유니티에서 제공하는 영구 데이터 디렉토리 경로입니다.  
자세한 설명은 [유니티 개발자 문서](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html)를 참고해 주세요.  

| OS      | 저장 경로(Application.persistentDataPath/backend.dat)                      |
| :------ | :-------------------------------------------------------------------------- |
| Windows | C:/Users/**userprofile**/AppData/LocalLow/**회사명**/**게임명**/backend.dat |
| Mac     | ~/Library/Application Support/**회사명**/**게임명**/backend.dat             |
| Android | /Data/Data/**packagename**/backend.dat                                      |
| iOS     | /var/mobile/Containers/Data/Application/**guid**/Documents/backend.dat      |

- **회사명**은 유니티 PlayerSetting에서 지정한 Company Name입니다.  
- **게임명**은 유니티 PlayerSetting에서 지정한 Product Name입니다.  
- **userprofile**은 윈도우에 로그인한 계정명입니다.  
- **guid**는 랜덤하게 생성되는 파일명입니다.(ex. 055811B9-D125-41B1-A078-F898B06F8C58)
- Android 환경에서 외부 저장소에 게임 데이터를 저장하도록 설정한 경우 저장 경로가 변경될 수 있습니다.  

## 암호화

**특별한 경우가 아니면 ClientAppID를 변경하지 않는 것을 권장합니다.**  
뒤끝 파일시스템을 통해 저장된 데이터는 유니티의 뒤끝 인스펙터 창에 기입한 **ClientAppId를 이용하여 암호화**되어있습니다.  

> 유니티 뒤끝 인스펙터 창에서 ClientAppID가 변경된 경우 기존에 저장되어 있던 backend.dat에 접근하지 못하게 됩니다.  
> 이 과정에서 기존에 저장되어 있던 데이터를 삭제하고 새 ClientAppID를 이용하여 암호화 한 backend.dat를 생성합니다.  
> **이 경우 로컬에 저장 중이던 유저의 토큰 정보, 차트 정보, 게스트 로그인 정보 등이 모두 삭제됩니다.**  

## 데이터 불러오기

뒤끝 기능을 호출할 때 필요한 경우 뒤끝 파일시스템 기능을 호출하여 데이터를 불러옵니다.  
뒤끝 차트 기능을 제외하고 개발자가 임의로 데이터를 불러올 수 없습니다.  

- backend.dat에 저장된 뒤끝 차트의 경우 개발자가 임의로 데이터를 불러올 수 있습니다.  

## 데이터 저장하기

뒤끝 기능을 호출할 때 필요한 경우 뒤끝 파일 시스템 기능을 호출하여 데이터를 저장합니다.  
개발자가 임의의 데이터를 저장할 수 없습니다.  
