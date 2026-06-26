---
sidebar_label: 설치
---

# 1대1문의 UI

:::info SDK 5.9.5 이상 지원 안내
해당 SDK를 사용하기 위해서는 뒤끝 SDK 5.9.5 이상의 버전이 설치되어 있어야 합니다.   

또한 UI 내부 데이터가 정상적으로 보이기 위해서는 뒤끝 로그인이 무조건 진행되어야합니다.  
:::

:::caution 파일첨부 기능 미제공 안내
현재 파일첨부 기능은 제공되지 않습니다. 해당 기능은 추후 업데이트를 통해 제공될 예정입니다
:::

## 1대1문의 UI 소개

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/main.png" />

1대1문의 UI란 유니티 UGUI를 이용하여 만든 1대1문의 기능이 구현된 UI입니다.  
뒤끝에서 제공하는 SDK를 기반으로 제작되었으며, 뒤끝 SDK가 설치되어있지 않을 경우 이용이 불가합니다.  

해당 오브젝트에 사용된 UI 및 스크립트는 원하시는 대로 자신의 게임에 맞게 수정하실 수 있습니다.  

## 1대1문의 설치법

### 1. 뒤끝 SDK 다운로드 및 유니티에 import

[개발자 문서 - 시작하기](/sdk-docs/backend/base/start-up)에서 Backend-5.9.5.unitypackage 이상의 SDK를 다운받습니다.  
이미 뒤끝 SDK를 사용중이시라면 2번으로 넘어가주세요.  

### 2. 1대1문의 UI SDK 다운로드 및 유니티에 import

- [BackendPlus-QuestionUI-1.0.0.unitypackage](https://developer.thebackend.io/sdk/unityUI/Question/BackendPlus-QuestionUI-1.0.0.unitypackage)&nbsp;\[2023-04-25]

위 링크를 통해 unityPackage 파일을 다운받고 프로젝트에 import합니다.  

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/BackendPlus.png" />

설치가 정상적으로 진행되었다면 Assets 폴더 내에 BackendPlus란 폴더가 생성되어있으며 해당 폴더 내부에는 1대1문의 UI를 구성하는 오브젝트 및 스크립트들이 존재합니다.  

실제 프로젝트에서 사용되는 UI 오브젝트는 `Assets > BackendPlus > UI > Question > Resource > BackendUI > Question`에 위치하는 QuestionUI를 사용합니다.  

### 3. TextMeshPro 임포트(없을 경우에만)

1대1 문의 UI에서는 모든 텍스트가 TextMeshPro로 설정되어있습니다.  

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/textmesh-1.png" />

유니티 상단 WIndow > PackageManager를 클릭하여 PackageManager를 엽니다.  

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/textmesh-2.png" />
카테고리를 Packages:Unity Registry로 변경하고 TextMeshPro를 찾은 후 install 합니다.  

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/textmesh-3.png" />

install이 정상적으로 완료되면 Assets 폴더 안에 TextMesh Pro라는 폴더가 생성됩니다.  

### 4. 해당 씬에 EventSystem 생성

1대1문의 UI는 UGUI로 구성되어있으며 해당 씬에 EventSystem 오브젝트가 있어야 클릭, 드래그등의 상호작용을 할 수 있습니다.  

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/eventsystem.png" />

유니티 Hierachy 창에서 우클릭을 통해 EventSystem을 생성합니다.  

> EventSystem이 한 씬에 두개 이상 존재할 경우, 에러가 발생하므로 주의해주시기 바랍니다.  

### 5. 게임 해상도 설정

1대1문의 UI는 1080 x 1920 해상도(가로)와 1920 x 1080 해상도(세로)를 기준으로 제작되었습니다. 16:9 혹은 9:16 비율로 설정하는 것을 권장드립니다.  

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/screensize.png" />

Game 뷰에서 해상도를 원하는 대로 설정합니다.  

### 6. 테스트 스크립트 작성

게임이 시작되면 UI가 생성되는 간단한 스크립트를 작성합니다.  

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/screensize.png" />

아무 폴더에나 스크립트를 하나 생성하고, Hierachy에 오브젝트를 하나 생성하여 스크립트를 추가합니다.  

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/script.png" />

해당 스크립트에는 다음과 같이 뒤끝 초기화와 뒤끝 로그인이 성공하면 UI를 활성화하는 스크립트를 작성합니다.  

:::danger 뒤끝 초기화 시 AsyncPoll
1대1문의 UI에서는 뒤끝 함수를 비동기 형태로 사용하기 때문에 AsyncPoll이 true일 경우에는 Update에서 Backend.AsyncPoll()을 지속적으로 호출해야합니다.  
 AsyncPoll이 false일 경우에는 내부 비동기 처리 로직으로 인해 별도 처리 없이도 정상적으로 UI가 작동합니다.  
:::

```js
using UnityEngine;
using BackEnd;
public class NewBehaviourScript : MonoBehaviour
{
    // Start is called before the first frame update
    async Start() {
        ar initResult = await BackndBase.InitializeAsync();
        if (!initResult.IsSuccess())
        {
            Debug.LogError("초기화에 실패했습니다. : " + initResult);
        }

        string id = "user1";
        string pw = "user1";

        // bro = Backend.BMember.CustomSignUp(id, pw);
        var loginResult = await BackndAuth.Instance.SignInCustomAsync("test1", "test2");
        if(loginResult.IsSuccess() == false)
        {
            Debug.LogError("로그인중 에러가 발생하였습니다. : " + loginResult);
            return;
        }

        BackendPlus.Question.OpenUI();
    }

    void Update() {

        // Initialize에서 userAsyncPoll이 false일 경우에는 작동하지 않아도 된다.  
        if(Backend.UseAsyncQueuePoll) {
            Backend.AsyncPoll();
        }
    }
}
```

### 7. 테스트

<img src="https://developer.thebackend.io/static/img/ToolKit/UnityUI/Question/test.png" />

유니티 실행 버튼을 눌러 테스트를 실행합니다.  
