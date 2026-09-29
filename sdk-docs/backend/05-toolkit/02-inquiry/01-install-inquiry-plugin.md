---
sidebar_label: "1대1 문의 플러그인"
description: "Unity에서 문의 버튼을 연결하고 1대1 문의 창 열기"
---

# 1대1 문의 플러그인

게임에 문의 버튼을 추가할 수 있습니다. 아래 순서대로 **플러그인 설치 → 코드 복사 → 버튼 연결**을 진행하세요.

![1대1 문의 창 예시](/img/docs/guide/toolkit/question/sample.png)

:::info 시작하기 전에
- 뒤끝 SDK 5.0.3 이상을 설치하고, 게임에서 뒤끝 초기화와 로그인이 완료되어 있어야 합니다.
- Android 7.0(API 24) 이상과 iOS 8.0 이상을 지원합니다.
- 문의 창은 **실제 Android/iOS 기기에서 확인**하세요. Unity 에디터에서는 열리지 않습니다.
:::

## 1. 플러그인 설치

사용할 플랫폼의 패키지를 다운로드한 뒤 Unity에 끌어다 놓고 **Import**를 누릅니다. 파일 선택은 모두 유지하세요.

**Android용** : [BackendQuestion-Android-1.2.1.unitypackage](https://developer.thebackend.io/sdk/question/BackendQuestion-Android-1.2.1.unitypackage) \[2026-09-29]  
**iOS용** : [BackendQuestion-iOS-1.0.1.unitypackage](https://developer.thebackend.io/sdk/question/BackendQuestion-iOS-1.0.1.unitypackage) \[2026-09-29]

설치된 `Assets/StreamingAssets/TheBackend/QuestionHtml/` 폴더는 이동하지 마세요.

### 기존 버전에서 업데이트하는 경우

- 직접 수정한 HTML·CSS 또는 iOS 권한 문구는 별도로 보관합니다.
- 새 패키지의 파일을 모두 임포트합니다. 공통 HTML·CSS와 Android의 `Editor/BackendQuestionDependencies.xml`도 포함해야 합니다.
- 이전 문의 플러그인이 설치한 `Assets/Plugins/Android/androidx.core.core-1.0.0.aar`가 남아 있다면 Unity의 Project 창에서 해당 파일을 삭제합니다. Unity가 `.meta`도 함께 삭제합니다. **다른 플러그인의 파일까지 삭제하지 마세요.**
- 아래 Android 설정까지 마친 뒤, 보관한 수정 내용을 새 파일에 반영합니다.

### Android 설정

**Android로 빌드할 때만** 진행합니다. 아래 메뉴 위치는 Unity 2022.3 기준입니다.

1. 빌드 플랫폼을 **Android**로 선택합니다.
2. **Assets → External Dependency Manager** 메뉴가 있는지 확인합니다. 문의 창에 필요한 Android 라이브러리를 준비하는 도구입니다. 메뉴가 없다면 Unity 2022.3 이상은 [Unity 공식 EDM](https://docs.unity3d.com/Packages/com.unity.external-dependency-manager@2.1/manual/get-started-with-edm.html), 그 이전 버전은 [Google EDM4U](https://github.com/googlesamples/unity-jar-resolver#getting-started)의 설치 안내를 따릅니다. 이미 설치되어 있다면 중복 설치하지 않습니다.
3. **Player Settings → Android → Other Settings → Target API Level**을 **API 34 이상**으로 설정합니다. 목록에 없다면 Unity가 사용하는 Android SDK에 해당 SDK Platform을 설치합니다.
4. **Player Settings → Publishing Settings**에서 **Custom Main Gradle Template**과 **Custom Gradle Properties Template**을 켭니다. 생성된 `Assets/Plugins/Android/gradleTemplate.properties`에 `android.useAndroidX=true`가 있는지 확인하고, 없다면 한 줄 추가합니다.
5. **Assets → External Dependency Manager → Android Resolver → Force Resolve**를 실행합니다. 완료되면 아래 코드를 추가합니다. 오류가 표시되면 [문제 해결](#문제-해결)을 확인하세요.

### iOS 설정

iOS는 패키지를 임포트하면 필요한 WebKit 설정과 카메라·마이크 권한 문구가 빌드 시 자동으로 추가됩니다. 아래 코드를 추가한 뒤 iOS 기기에서 확인하세요.

## 2. 코드 복사

Unity에서 **`QuestionExample.cs`** 파일을 만들고, 내용을 아래 코드로 교체합니다. 버튼을 누르면 로그인한 사용자의 문의 창이 열립니다. 인증 코드는 코드에서 받아오므로 직접 입력할 필요가 없습니다.

```csharp
using BackEnd;
using UnityEngine;

#if UNITY_ANDROID && !UNITY_EDITOR
using QuestionPlugin = TheBackend.ToolKit.Question.Android;
#elif UNITY_IOS && !UNITY_EDITOR
using QuestionPlugin = TheBackend.ToolKit.Question.iOS;
#endif

public class QuestionExample : MonoBehaviour
{
    public void OpenQuestionView()
    {
#if (UNITY_ANDROID || UNITY_IOS) && !UNITY_EDITOR
        if (!Backend.IsInitialized || string.IsNullOrEmpty(Backend.UserInDate))
        {
            Debug.LogError("뒤끝 초기화와 로그인 후 문의 버튼을 눌러 주세요.");
            return;
        }

        var bro = Backend.Question.GetQuestionAuthorize();
        if (!bro.IsSuccess())
        {
            Debug.LogError("문의 인증에 실패했습니다. 로그인 상태와 네트워크를 확인해 주세요.");
            return;
        }

        string questionAuthorize = bro.GetReturnValuetoJSON()["authorize"].ToString();
        QuestionPlugin.OpenQuestionView(questionAuthorize, Backend.UserInDate, error =>
        {
            Debug.LogError("문의 창을 열지 못했습니다: " + error);
        });
#else
        Debug.LogWarning("문의 창은 Android/iOS 기기에서만 열 수 있습니다.");
#endif
    }
}
```

이 코드는 기존 게임의 초기화·로그인을 사용합니다. 초기화나 로그인을 대신 실행하지 않으며, 인증 코드가 포함된 응답은 로그로 출력하지 않습니다.

## 추가 설정

기본 문의 창만 사용한다면 아래 설정은 건너뛰어도 됩니다.

### 문의 창 여백과 닫기 버튼 크기 변경

위 코드의 `QuestionPlugin.OpenQuestionView(...)` 바로 앞에 다음 코드를 추가합니다.

```csharp
var layout = new QuestionPlugin.QuestionViewLayout();
layout.leftMargin = 5;
layout.topMargin = 5;
layout.rightMargin = 5;
layout.bottomMargin = 5;
layout.buttonWeight = 1;
layout.viewWeight = 15;
QuestionPlugin.SetQuestionViewLayout(layout);
```

여백의 기본값은 모두 `0`입니다. `buttonWeight`와 `viewWeight`는 닫기 버튼 영역과 문의 영역의 세로 비율이며, 기본값은 `1 : 11`입니다. 예제의 `1 : 15`는 닫기 버튼 영역을 더 작게 만듭니다. 여백은 0 이상, 두 비율은 양수로 설정합니다.

### 코드로 닫기 또는 닫힘 알림 받기

- 코드로 닫기: `QuestionPlugin.CloseQuestionView()`를 호출합니다.
- 닫힘 알림 받기: 창을 열기 전에 `QuestionPlugin.SetCloseQuestionViewCallback(() => Debug.Log("문의 창이 닫혔습니다."));`를 호출합니다. 창이 닫히면 로그가 출력됩니다.

여기서 `QuestionPlugin`은 위 예제에서 플랫폼에 맞게 선택한 이름입니다. 위 예제와 동일하게 Android/iOS 기기 실행 조건 안에서 사용하세요. 플러그인 함수는 Unity 메인 스레드에서 호출하고, 콜백에서 Unity UI를 변경할 때도 메인 스레드로 전달해 처리해야 합니다.

### iOS 카메라·마이크 권한 문구 변경

`Assets/TheBackend/ToolKit/Question/iOS/Editor/BackendQuestionProcessBuildForIOS.cs`에서 다음 문자열을 수정한 뒤 다시 빌드합니다.

- `cameraMessage`: 카메라 사용 안내 문구(`NSCameraUsageDescription`)
- `videoMessage`: 동영상 촬영 시 마이크 사용 안내 문구(`NSMicrophoneUsageDescription`)

### Android 난독화(ProGuard)를 사용하는 경우

ProGuard 규칙에 다음 줄을 추가합니다.

```text
-keep class io.thebackend.questionwebview.** {*;}
```

## 문제 해결

| 증상 | 확인할 내용 |
| --- | --- |
| Unity 에디터에서 창이 열리지 않음 | 에디터에서는 지원하지 않습니다. 실제 Android/iOS 기기로 빌드하세요. |
| 버튼을 눌러도 반응이 없음 | 버튼의 `On Click()`에 QuestionManager와 `OpenQuestionView()`가 연결되어 있는지 확인하세요. |
| 초기화·로그인 또는 문의 인증 오류 | 뒤끝 초기화와 로그인이 성공했는지, 네트워크에 연결되어 있는지 확인하세요. |
| `External Dependency Manager` 메뉴가 없음 | [Android 설정](#android-설정)의 2번 설치 단계를 확인하세요. |
| `WebViewAssetLoader` 오류 또는 Android 문의 창 종료 | `Force Resolve`를 실행한 뒤 앱을 다시 빌드하세요. 계속 실패하면 아래 항목을 확인하세요. |
| `duplicateClasses` 오류 | 이전 문의 플러그인의 `androidx.core.core-1.0.0.aar`가 남아 있는지 확인하세요. 다른 플러그인의 파일은 임의로 삭제하지 마세요. |
| API 34 또는 `compileSdk` 오류 | Android SDK Platform 34 이상을 설치하고 `Target API Level`도 34 이상으로 설정하세요. |

### Force Resolve 후에도 Android 빌드가 실패하는 경우

1. `Assets/TheBackend/ToolKit/Question/Android/Editor/BackendQuestionDependencies.xml`이 있는지 확인합니다. 없다면 문의 플러그인을 다시 임포트합니다.
2. `Assets/Plugins/Android/mainTemplate.gradle`의 `Android Resolver Dependencies` 영역에 `androidx.core:core:1.12.0`과 `androidx.webkit:webkit:1.9.0`이 반영되어 있는지 확인합니다. **같은 항목을 수동으로 중복 추가하지 마세요.** 다른 플러그인이 더 높은 버전을 요구하면 선택된 버전의 빌드 환경 요구사항도 확인합니다.
3. 기존 프로젝트에서 Jetifier를 사용한다면 `gradleTemplate.properties`의 `android.enableJetifier=true` 설정이 유지되어 있는지 확인합니다.

문의 플러그인의 AAR 파일만 복사해서는 필요한 Android 라이브러리가 함께 설치되지 않습니다. Android 1.2.1의 파일 선택 기능을 위해 저장 공간 권한을 별도로 추가할 필요는 없습니다.
