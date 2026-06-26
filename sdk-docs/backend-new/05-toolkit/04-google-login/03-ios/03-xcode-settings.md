# XCode 설정(빌드,실행 에러 시)

:::info iOS에서 구글 로그인을 진행하려면 다음과 같은 절차가 이루어져야 합니다. 
1. 유니티에서 Xcode로 빌드  
2. CoCoaPod을 이용하여 GoogleSignIn SDK 를 XCode에 추가  
:::

## 1. 유니티에서 XCode로 프로젝트 빌드  
유니티에서 Platform iOS를 선택한 후, Build를 클릭합니다.  

## 2. Cocoapod 설치  
1) mac 내 terminal을 켜서 다음과 같은 명령어를 입력합니다.  
```js
> sudo gem install cocoapods
```

2) PC비밀번호 입력합니다. 다음과 같은 문구가 출력되면 설치가 완료된 것입니다.  
```js
Successfully installed cocoapods
Parsing documentation for cocoapods
Done installing documentation for cocoapods after 1 seconds
1 gem installed
```

## 2. Podfile 생성  

1) 유니티에서 빌드한 프로젝트의 폴더로 이동합니다.  
```js
// 예시
> cd Users/username/MyUnityProject/ios_Build
```

2) 폴더 내에 PodFile이 존재하는지 확인합니다.  
PodFile이 존재하지 않는다면 명령어 pod init을 입력하여 PodFile을 생성합니다.  
**Firebase를 이용할 경우 PodFile이 자동으로 생성될 수 있습니다.**  

```js
> pod init
```

3) 생성된 PodFile을 텍스트 편집기로 열어 **pod 'GoogleSignIn', '7.1.0'** 을 입력합니다.  

![](/img/docs/guide/toolkit/google-login/pod-install.png)


4) PodFile을 저장한 후,  터미널에 'pod install'을 입력하여 xcworkspace을 생성합니다.  
생성이 성공하였다면 다음과 같은 문구가 출력됩니다.  

![](/img/docs/guide/toolkit/google-login/pod-success.png)

## 3. 앱 빌드  
이후 앱을 빌드하여 구글 로그인이 정상적으로 되는 작동하는 것을 확인합니다.  
<img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/result.png" />


## Error Case. 앱이 중단될 경우  
xcodeproj 파일에서 Info 항목에 GIDClient 혹은 URL Types가 제대로 설정되어있지 않을 경우, 함수 호출 시 앱이 중단될 수 있습니다.  
해당 경우에는 수동으로 iOS Schema를 설정합니다.

1) 생성된 Unity-iPhone.xcworkspace을 엽니다.  

### GIDClient
Unity-iPhone > Info > Custom iOS Target Properties에서 GIDClientID를 추가하고, Type을 String으로 설정합니다.  
Value는 구글 클라우드 플랫폼에서 생성한 클라이언트 ID를 입력합니다.
![](/img/docs/guide/toolkit/google-login/gid-client.png)

### URL Types
1) Unity-iPhone > Info > URL Types 를 열고 + 버튼을 누릅니다.  
<img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/pod-3.png" />

2) 생성된 URL 입력창 중 URL Schemes 부분에 구글 클라우드 플랫폼에서 복사한 iOS URL 스키마를 붙여넣습니다.  
> <img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/ios-id-2.png" />
<img src="https://developer.thebackend.io/static/img/ToolKit/googleLogin/iOS/pod-4.png" />

