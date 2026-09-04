---
description: "시작하기"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 시작하기

### SDK 연동 과정을 영상으로 확인하세요!

지금 영상을 보면서 쉽고 빠르게 SDK 연동을 완료하실 수 있습니다.

<div className="sdk-tutorial-video">
  <iframe src="https://www.youtube.com/embed/sruvB7QpG1U?enablejsapi=1" allowfullscreen></iframe>
</div>

:::info 다음 단계를 따라 뒤끝 SDK 연동을 시작해 보세요.
* 최신 뒤끝 SDK 다운로드
* 다운로드한 SDK를 Unity에 설치
* Unity Project에 뒤끝 인증 정보 적용
* 뒤끝 초기화 Script 작성
* 테스트, 완료!
:::

## Step1. 최신 뒤끝 SDK 다운로드

아래 뒤끝 SDK를 다운로드합니다. 뒤끝 베이스 SDK는 베이스 기능 외, 뒤끝매치 기능도 포함하고 있습니다.
- <a href="https://developer.thebackend.io/sdk/unityPackage/5.18.16/Backend-5.18.16.unitypackage" id="download-sdk">Backend-5.18.16.unitypackage</a>&nbsp;[2026-09-04]

**최초 설치인 경우**
- Step2.로 이동합니다.

**버전 업그레이드인 경우**
- SDK 업데이트 시 유니티의 The Backend Settings 인스펙터 창에 설정한 뒤끝 인증값이 초기화될 수 있습니다.    
**SDK 업데이트 후 초기화가 되지 않는다면 인스펙터 창의 인증값 확인을 부탁드립니다.**
- 버전 업그레이드 진행 시, 반드시 업그레이드하고자 하는 신규 SDK의 [업데이트 노트](/sdk-docs/backend/base/update-history)를 확인해 주세요.
- 버전 업그레이드에 관하여 문제 발생 시, [뒤끝 커뮤니티](https://community.thebackend.io/)를 통해 문의 남겨 주시면 신속히 확인하겠습니다.

## Step2. 다운로드한 SDK를 Unity에 설치

1. Unity Editor를 열고 Project 창을 활성화합니다. 그리고 다운로드 한 뒤끝 SDK를 Project 창 화면 위에 드래그 앤 드롭합니다.  
또는 `Unity 상단 메뉴 > Assets > Import Package > Custom Package`를 열고 다운로드 한 뒤끝 SDK를 선택해 Import 할 수 있습니다.

 &emsp;&emsp;![start-up 1](/img/docs/guide/start-up/import-package.png)

2. import Unity Pakage 창이 나타나면, 모든 항목을 체크하고 Import 버튼을 클릭합니다.

 &emsp;&emsp;![start-up 2](/img/docs/guide/start-up/import-package2.png)

3. 뒤끝 SDK가 잘 설치되었다면 Unity 상단 메뉴에 `The Backend` 가 추가된 것을 확인할 수 있습니다.

 &emsp;&emsp;![start-up 3](/img/docs/guide/start-up/import-package3.png)


## Step3. Unity Project에 뒤끝 인증 정보 적용

1. 뒤끝 콘솔에서 게임 인증 정보를 발급합니다.  
게임 인증 정보는 **Client App Id**와 **Signature Key**로 구성되어 있으며, 콘솔에서 프로젝트를 생성하는 즉시 발급됩니다.  
콘솔의 `프로젝트 설정 > 인증 정보`에서 확인할 수 있습니다.

 &emsp;&emsp;<ConsoleLinkButton text="인증 정보 바로가기" menu="settingAuth" feature="인증 정보" title="인증 정보" />

 &emsp;&emsp;![start-up 4](/img/docs/guide/start-up/console-client-app-id.png)

2. Unity Inspector에 발급한 인증 정보를 적용합니다.  
Unity Inspector는 `Unity 상단 메뉴 > The Backend > Edit Settings` 을 클릭해 활성화할 수 있으며,  
뒤끝 콘솔에서 복사한 **ClientApp Id**와 **Signature Key**를 아래와 같이 붙여 넣어줍니다.

 &emsp;&emsp;![start-up 5](/img/docs/guide/start-up/unity-edit-settings.png)

## Step4. 뒤끝 초기화 Script 작성

1. Script를 생성합니다. Unity Project 창에서 우클릭 후 `Create > Scripting > MonoBehavior Script`를 클릭해 생성할 수 있습니다.

 &emsp;&emsp;![start-up 6](/img/docs/guide/start-up/create-script.png)

2. 생성된 Script를 우클릭하여 Rename 선택 후, 원하는 이름으로 변경합니다. 여기서는 BackendManager로 변경하겠습니다.

 &emsp;&emsp;![start-up 6-2](/img/docs/guide/start-up/script-rename.png)

3. 이름을 변경해 준 스크립트를 더블클릭해 코드 편집기를 엽니다.

:::tip TIP! 코드 편집기가 열리지 않는다면 확인해 보세요!
`Unity 상단 메뉴 > Edit > Preferences`를 눌러 창을 활성화하고, External Tools 창에서 External Script Editor를 설정합니다.

![start-up 7](/img/docs/guide/start-up/unity-external-tools.png)
:::

4. 뒤끝 초기화 Script를 작성합니다.  
기본 설정된 초기 Script는 다음과 같으며, 해당 코드에 뒤끝 적용 Script를 복사 붙여넣기 해 덮어씌워 줍니다.

 **초기 Script**

 ```js
 using System.Collections;
 using System.Collections.Generic;
 using UnityEngine;

 public class BackendManager : MonoBehaviour
 {
     // Start is called before the first frame update
     void Start()
     {

     }

     // Update is called once per frame
     void Update()
     {

     }
 }
 ```

 **뒤끝 적용 Script**

 ```js
 using System.Collections;
 using System.Collections.Generic;
 using UnityEngine;

 // 뒤끝 SDK namespace 추가
 using BackEnd;

 public class BackendManager : MonoBehaviour {
     void Start() {
         var bro = Backend.Initialize(); // 뒤끝 초기화

         // 뒤끝 초기화에 대한 응답값
         if(bro.IsSuccess()) {
             Debug.Log("초기화 성공 : " + bro); // 성공일 경우 statusCode 204 Success
         } else {
             Debug.LogError("초기화 실패 : " + bro); // 실패일 경우 statusCode 400대 에러 발생
         }
     }
 }
 ```

5. GameObject를 생성하고, 작성한 뒤끝 적용 Script를 부착합니다.  
GameObject는 Unity Hierachy 창에서 우클릭 후 `Create Empty`를 클릭하여 생성할 수 있습니다.  
이후 생성된 GameObject를 클릭해, GameObject를 가리키는 Inspector에 뒤끝 초기화 Script를 드래그 앤 드롭합니다.

 &emsp;&emsp;![start-up 8](/img/docs/guide/start-up/attach-script.png)

 **Script를 Inspector에 부착한 모습**
 &emsp;&emsp;![start-up 9](/img/docs/guide/start-up/attach-script2.png)

6. 초기화를 실행할 준비가 모두 완료되었습니다.

## Step5. 테스트, 완료!

1. 테스트를 진행합니다. Unity 상단 가운데 있는 플레이 버튼(▶)을 눌러 주세요. 이후 Unity Console 창에서 초기화 성공 문구를 확인합니다.  
`초기화 성공 : statusCode : 204` 로그가 나온다면 세팅이 완료된 것입니다.

 &emsp;&emsp;![start-up 10](/img/docs/guide/start-up/unity-success.png)

2. 이제 뒤끝의 모든 기능을 정상 이용할 수 있습니다!

:::info 초기화가 완료되었나요?
뒤끝의 어떤 기능을 가장 먼저 구현해야 할지 선택이 어려우시다면, [**뒤끝 가이드라인**](/sdk-docs/backend/base/guideline/new-user/before)을 따라 해 보세요.  
뒤끝을 사용하기 위해 **반드시 구현되어야 하는 기본 기능**(회원가입, 로그인)부터, 여러 게임 콘텐츠에 필요한 핵심 기능들이 정리되어 있습니다.
:::
