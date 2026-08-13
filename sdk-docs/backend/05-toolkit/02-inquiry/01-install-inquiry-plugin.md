---
sidebar_label: "1대1 문의 플러그인"
description: "1대1 문의 플러그인 설치"
---

# 1대1 문의 플러그인 설치  

## 1대1 문의 플러그인

Android : [BackendQuestion-Android-1.2.0.unitypackage](https://developer.thebackend.io/sdk/question/BackendQuestion-Android-1.2.0.unitypackage) \[2026-02-27]  
iOS : [BackendQuestion-iOS-1.0.0.unitypackage](https://developer.thebackend.io/sdk/question/BackendQuestion-iOS-1.0.0.unitypackage) \[2024-03-26]


1대1문의 기능은 **Android, iOS만 지원**하고 있으며, 유니티 에디터에서는 작동하지 않습니다.  
Android는 **7.0(API-24) 이상 버전**에서만, iOS는 **8.0+ 이상 버전**에서만 지원하고 있으며, 이전 버전은 지원하고 있지 않습니다.  

1대1 문의하기 기능은 SDK 5.0.3 이상 버전에서 위 플러그인을 추가로 설치한 경우 사용할 수 있습니다.  

> BackendQuestion 용 html 및 css 파일이 담긴 StreamingAssets/TheBackend/QuestionHtml/ 폴더를 이동시킬 경우 비정상적으로 동작됩니다.   
해당 폴더의 경로는 고정시켜 주세요.  


![](/img/docs/guide/toolkit/question/sample.png)



## 플랫폼별 추가사항

임포트 시, 유니티에 해당 파일들이 생성됩니다.  

### 공통

- BackendQuestionMain.html
- BackendQuestionList.html
- BackendQuestionStyle.css
  >(Assets/StreamingAssets/TheBackend/QuestionHtml/ 세 파일 모두 동일한 경로입니다.)

### Android

:::warning proguard 안내
프로가드를 이용중이실 경우 다음 예외 처리가 필요합니다.  
```
-keep class io.thebackend.questionwebview.** {*;}
```
:::




- io.thebackend.questionwebview.aar(Assets/TheBackend/ToolKit/Question/Android/)
- TheBackend.ToolKit.Question.Android.dll(Assets/TheBackend/ToolKit/Question/Android/)
- androidx.core.core-1.0.0.aar(Assets/Plugins/Android/)
  Android 빌드 시, Assets/Plugins/Android/안에 다른 버전의 androidx.core.core.aar이 있을 경우에는 최신 버전을 남겨두고 오래된 버전을 삭제해 주세요.  
  > 빌드 시 앱에 저장 공간 권한 설정이 추가되며, 문의 창 내에 첨부파일 추가를 할 경우 저장 공간 권한이 활성화됩니다.  
  > 최종 AndroidManifest.xml에 자동으로 android.permission.WRITE_EXTERNAL_STORAGE와 android.permission.READ_EXTERNAL_STORAGE가 추가됩니다.  



### iOS

- BackendQuestionProcessBuildForIOS.cs(Assets/TheBackend/ToolKit/Question/iOS/Editor/)
- TheBackend.ToolKit.Question.iOS.dll(Assets/TheBackend/ToolKit/Question/iOS/)
- BackendQuestionViewForIOS.mm(Assets/TheBackend/ToolKit/Question/iOS/)
  > 빌드 후 생성된 Xcode Project 내 UnityFramework에 WebKit.framework가 required로 추가됩니다.  
  > 사진 및 카메라 및 비디오 촬영 기능 사용 시 권한 문구에 대한 정의 NSCameraUsageDescription, NSMicrophoneUsageDescription가 info.plist에 추가됩니다.  
  > 해당 문구는 1대1 문의 창 내 첨부파일 추가 - Take Photo or Video 기능 선택 시 사용됩니다.  
  > 해당 문구는(Assets/Editor/TheBackend/BackendQuestionProcessBuildForIOS.cs) 스크립트에서 cameraMessage와 videoMessage를 통해 변경할 수 있습니다.  

## Error cases

임포트 이후, 빌드 또는 실행 시에 해당 에러가 발생할 수 있습니다.  

**Android 유니티 빌드 시, duplicateClasses 오류**  
androidx.core.core.aar 플러그인이 2개 이상 있을 경우 발생하는 에러입니다.  
만약 GPGS 플러그인이나 Admob 플러그인을 임포트 했을 경우 aar 파일이 중복될 수 있으니 오래된 버전의 androidx.core.core.aar을 찾아 제거해 주시기 바랍니다.  

> androidx.core.core.aar 파일이 프로젝트 내 1개만 존재해야 합니다.  

**Android 1대1 문의 창에서 첨부파일 추가 클릭 시, 게임이 꺼지는 오류**  
androidx.core.core.aar가 빌드에 임포트되지 않아 생기는 현상입니다.  
Plugins 폴더에 androidx.core.core.aar이 존재하는지 확인해 주시기 바랍니다.  

---

# 예제 코드

## 1:1 문의창 인증정보 불러오기

### 설명
Android, iOS 네이티브 코드에서 이루어지는 1:1 문의에 필요한 인증코드를 발급받습니다.  
해당 함수는 뒤끝베이스에 포함되어 있으며, 로그인이 이루어진 이후에 호출 가능합니다.  

### Example
```js
// 동기 함수
var bro = Backend.Question.GetQuestionAuthorize();
string questionAuthorize = bro.GetReturnValuetoJSON()["authorize"].ToString();

// 비동기 함수
Backend.Question.GetQuestionAuthorize(callback =>
{
    string questionAuthorize = callback.GetReturnValuetoJSON()["authorize"].ToString();
});
```


## 1:1 문의창 열기

### 설명
유니티 뷰 위에 1:1 문의 뷰를 생성합니다.  


### 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| questionAuthorize | string | Backend.Question.GetQuestionAuthorize 함수를 통해 발급받은 인증 코드 |
| myInDate | string | 자기 자신의 유저 inDate(Backend.UserInDate 고정) |
| questionOnErrorCallback | QuestionOnErrorCallback | 문의창 생성 중 에러가 발생했을 때 작동하는 핸들러 |
> public delegate void QuestionOnErrorCallback(string error);

### Example

```js
var bro = Backend.Question.GetQuestionAuthorize();
string questionAuthorize = bro.GetReturnValuetoJSON()["authorize"].ToString();

#if UNITY_ANDROID
TheBackend.ToolKit.Question.Android.OpenQuestionView(questionAuthorize, Backend.UserInDate, (error) =>
{
    Debug.LogError("1:1 문의창 활성화중 에러가 발생했습니다 : " + error);
});

#elif UNITY_IOS
TheBackend.ToolKit.Question.iOS.OpenQuestionView(questionAuthorize, Backend.UserInDate, (error) =>
{
    Debug.LogError("1:1 문의창 활성화중 에러가 발생했습니다 : " + error);
});
#endif
```


## 1:1 문의창 레이아웃 설정하기

### 설명
1:1 문의창의 레이아웃을 설정합니다.  
상하좌우 여백과 버튼과 문의창의 비율을 설정할 수 있습니다.
X 버튼의 크기를 줄이고 싶을 경우, buttonWeight = 1, viewWeight는 11 보다 크게 설정해야합니다.  

### QuestionViewLayout 클래스
| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :---- |
| leftMargin | int | Backend.Question.GetQuestionAuthorize 함수를 통해 발급받은 인증 코드 | 0 |
| topMargin | int | Backend.Question.GetQuestionAuthorize 함수를 통해 발급받은 인증 코드 | 0 |
| rightMargin | int | Backend.Question.GetQuestionAuthorize 함수를 통해 발급받은 인증 코드 | 0 |
| bottomMargin | int | Backend.Question.GetQuestionAuthorize 함수를 통해 발급받은 인증 코드 | 0 |
| buttonWeight | int | 전체 세로 비율(buttonWeight + viewWeight) 중 버튼 뷰가 포함되어 있는 뷰의 비율 | 1(1 : 12) |
| viewWeight | int | 전체 세로 비율 중(buttonWeight + viewWeight) 문의창 뷰가 포함되어 있는 뷰의 비율  | 11(11 : 12) |

> buttonWeight = 2, viewWeight = 18, 화면 세로 크기가 1800 이라면 일 경우  
버튼 레이아웃의 세로는 1800 x 2 / (2+18), 문의 레이아웃의 세로는 1800 x 18 / (2+18)  

### Example
```js

#if UNITY_ANDROID
TheBackend.ToolKit.Question.Android.QuestionViewLayout
    questionViewLayout = new Android.QuestionViewLayout();
#elif UNITY_IOS
TheBackend.ToolKit.Question.iOS.QuestionViewLayout
    questionViewLayout = new iOS.QuestionViewLayout();
#endif

TheBackend.ToolKit.Question.Android.QuestionViewLayout
    questionViewLayout = new Android.QuestionViewLayout();
questionViewLayout.leftMargin = 5; // 왼쪽 여백
questionViewLayout.topMargin = 5; // 위쪽 여백
questionViewLayout.rightMargin = 5; // 오른쪽 여백
questionViewLayout.bottomMargin = 5; // 아래쪽 여백

// button이 있는 검은색 레이아웃과 문의 레이아웃의 비율
// 버튼 레이아웃이 세로의 1할, 문의 레이아웃이 9할을 차지
// 화면 세로 크기가 1000이라면 button 레이아웃의 세로는 100, 문의 레이아웃은 900
questionViewLayout.buttonWeight = 1;
questionViewLayout.viewWeight = 9;

#if UNITY_ANDROID
TheBackend.ToolKit.Question.Android.SetQuestionViewLayout(questionViewLayout);
#elif UNITY_IOS
TheBackend.ToolKit.Question.iOS.SetQuestionViewLayout(questionViewLayout);
#endif

```


## 1:1 문의창 닫기

### 설명
1:1 문의창을 종료합니다.

### Example
```js
#if UNITY_ANDROID
TheBackend.ToolKit.Question.Android.CloseQuestionView();
#elif UNITY_IOS
TheBackend.ToolKit.Question.iOS.CloseQuestionView();
#endif
```


## 1:1 문의창 닫기 핸들러

### 설명
1:1 문의창이 닫힐 때 호출되는 닫기 핸들러의 동작을 추가합니다.

### Example
```js
#if UNITY_ANDROID
TheBackend.ToolKit.Question.Android.SetCloseQuestionViewCallback(() =>
{
    Debug.Log("창이 닫혔습니다.");
});

#elif UNITY_IOS
TheBackend.ToolKit.Question.iOS.SetCloseQuestionViewCallback(() =>
{
    Debug.Log("창이 닫혔습니다.");
});
#endif
```

---

## SampleCode
```js
using UnityEngine;
using BackEnd;
using TheBackend.ToolKit.Question;

public class NewBehaviourScript : MonoBehaviour
{
    private string authCode = "";
    
    void Start()
    {
        if (!Backend.Initialize(true).IsSuccess())
        {
            Debug.LogError("초기화에 실패했습니다.");
        }

        var bro = Backend.BMember.CustomLogin("test1", "test2");

        if (bro.IsSuccess())
        {
            var bro2 = Backend.Question.GetQuestionAuthorize();
            authCode = bro2.GetReturnValuetoJSON()["authorize"].ToString();
        }
    }

    void OpenQuestionView()
    {
        #if UNITY_ANDROID
                TheBackend.ToolKit.Question.Android.QuestionViewLayout
            questionViewLayout = new Android.QuestionViewLayout();
        #elif UNITY_IOS
        TheBackend.ToolKit.Question.iOS.QuestionViewLayout
            questionViewLayout = new iOS.QuestionViewLayout();
        #endif

        questionViewLayout.buttonWeight = 1;
        questionViewLayout.viewWeight = 9;
        
        #if UNITY_ANDROID
        TheBackend.ToolKit.Question.Android.SetQuestionViewLayout(questionViewLayout);
        TheBackend.ToolKit.Question.Android.SetCloseQuestionViewCallback(() =>
        {
            Debug.Log("창이 닫혔습니다. 게임을 재개합니다.");
        });
        TheBackend.ToolKit.Question.Android.OpenQuestionView(authCode, Backend.UserInDate, (error) =>
        {
            Debug.LogError("1:1 문의창 활성화중 에러가 발생했습니다 : " + error);
        });
        
#elif UNITY_IOS
        TheBackend.ToolKit.Question.iOS.SetQuestionViewLayout(questionViewLayout);
        TheBackend.ToolKit.Question.iOS.SetCloseQuestionViewCallback(() =>
        {
            Debug.Log("창이 닫혔습니다. 게임을 재개합니다.");
        });
        TheBackend.ToolKit.Question.iOS.OpenQuestionView(authCode, Backend.UserInDate, (error) =>
        {
            Debug.LogError("1:1 문의창 활성화중 에러가 발생했습니다 : " + error);
        });  
#endif
    }
}
```
