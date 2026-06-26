# 뒤끝 구글 로그인 SDK

뒤끝 구글 로그인 SDK는 Andorid, iOS에서 페더레이션 로그인을 위해 사용하는 구글 로그인 기능을 더 쉽게 사용하도록
뒤끝에서 가공하여 제공하는 SDK입니다.

구글 클라우드 플랫폼에 업로드 키, sha-1 인증키를 등록하는 것만으로 콘솔 설정이 완료되며, 
SDK에서 간단한 핸들러를 등록하고 로그인 함수를 호출하는 것으로 쉽게 로그인이 가능해집니다.

뒤끝 SDK 버전에 상관없이 사용 가능합니다.

:::caution GPGS 안내  
해당 로그인 방식은 구글이 유니티에서 지원중인 GPGS 와 연동된 로그인 방식이 아닌 앱에서 주로 사용되는 구글 로그인 방식입니다.  
**GPGS 의 기능의 리더보드 혹은 업적등을 이용하실 경우에는 해당 플러그인을 사용하지 말고 GPGS 플러그인을 이용하여 로그인을 구현해 주시기 바랍니다.**  
:::  

:::danger Android용 SDK 3.0.0 마이그레이션 안내
안드로이드에서 인증 간소화 정책으로 인해 기존 구글 로그인이 인증관리자로 통합됩니다.  
- [Credential Manager replaces legacy APIs](https://android-developers.googleblog.com/2024/09/streamlining-android-authentication-credential-manager-replaces-legacy-apis.html)  

기존 API를 사용하는 로그인 방식은 2025년 상반기 이후에 지원이 종료될 예정입니다.  
인증관리자를 사용한 신규 구글 로그인을 사용하기 위해서는 SDK 3.0.0을 적용하셔야 합니다.  
SDK 3.0.0에서는 일부 API의 입력 인자가 변경되었으므로 코드 예제 페이지를 참고하시기 바랍니다.  
상기 내용은 안드로이드 구글로그인에만 해당하며 ios 구글 로그인은 관련 없습니다.  
   
- **기술적 제한 사항**  
구글 로그인 SDK 3.0은 **구글이 제공하는 신규 로그인 라이브러리**를 기반으로 동작합니다.  
따라서, 현재 발생 중인 오류는 **라이브러리 자체의 한계 및 Google Play Services 호환성 문제**에 기인하며, 뒤끝에서 직접 수정하거나 우회할 수 있는 부분이 제한적입니다.  
유니티 빌드 시, 안드로이드 Minimum API 레벨 조정 및 Google Play Services 버전을 최신 상태로 유지하는 것이 현재 가능한 유일한 대응 방법입니다.     
- **구글 로그인 SDK 2.2.0 버전 사용 임시 대응 안내**  
신규 로그인 방식에서 발생하는 문제로 인해 운영 부담이 크신 경우, [**기존 버전(2.2.0)**](https://developer.thebackend.io/sdk/google-login/BackendGoogleLogin-Android-2.2.0.unitypackage) 을 계속 사용해 주시고,  
추후 구글 측의 대응이 완료되거나 지원 종료 일정이 명확히 발표되는 경우, 3.0 버전으로 업데이트를 진행하셔도 무방합니다.
:::

:::danger 1.0.0 -> 2.0.0 이상 마이그레이션 안내
만약 1.0.0 버전에서 2.0.0으로 업그레이드를 할 경우, 기존 Assets > The Backend > Toolkit > GoogleLogin 폴더를 제거한 후, 2.0.0 버전을 import 해주시기 바랍니다.  

또한 기존에 유니티 인스펙터에 입력한 GCP의 webClientId, ios URL Schema, ios Client ID가 초기화 됩니다.  
꼭 한번 더 정보들을 **재입력**해주시기 바랍니다.
:::

## 구글 로그인 SDK 적용  
:::danger 적용 후 Google Play에 업로드하여 테스트를 꼭 해주세요!
구글 플레이에서는 앱 업로드 시, 업로드된 앱에 대하여 난독화, 보안, 무결성 검사, 최적화등의 작업이 자동으로 진행될 수 있습니다.  
이로 인해 유니티에서 빌드한 앱에서는 정상작동이 되더라도, 구글 플레이에서 다운받은 앱에서는 정상 작동이 되지 않을 수 있습니다.  

구글 로그인 SDK와 같은 외부 SDK를 import했을 경우에는 구글 플레이에서 내부 테스트 혹은 알파,베타 테스트에 앱을 업로드한 후 다운받는 실제 테스트가 필요합니다.  
:::

**Android용** :  
[BackendGoogleLogin-Android-3.0.1.unitypackage](https://developer.thebackend.io/sdk/google-login/android/BackendGoogleLogin-Android-3.0.1.unitypackage) \[2025-07-08]  
[BackendGoogleLogin-Android-2.2.0.unitypackage](https://developer.thebackend.io/sdk/google-login/BackendGoogleLogin-Android-2.2.0.unitypackage) \[2024-05-28]  
- [3.0.1][fix] 구글 로그인, 로그아웃에 대한 콜백에서 유니티 전용함수 사용시, 크래시가 발생하던 문제 수정.

**iOS용** : [BackendGoogleLogin-iOS-2.1.0.unitypackage](https://developer.thebackend.io/sdk/google-login/BackendGoogleLogin-iOS-2.1.0.unitypackage) \[2024-04-24]  


구글 로그인 SDK는 **Android, iOS만 지원**하고 있으며, 유니티 에디터에서는 작동하지 않습니다.  

## External Dependency Manager for Unity 설치
구글 로그인 SDK 2.0.0 버전 이후부터는 External Dependency Manager for Unity(이하 EDM4U)에 의해 안드로이드용 플러그인, iOS용 cocoapod 파일이 생성됩니다.  

EDM4U는 파이어베이스나 애드몹 등 서드파티 설치시에 함께 설치되는 경우가 많습니다.

유니티에서 Assets > ExternalDependencyManager 폴더가 있는지 확인해주세요.  

![](/img/docs/guide/toolkit/google-login/asset-edm4u.png)

만약 해당 폴더가 존재하지 않는 다면 아래 링크에서 다운 받을 수 있습니다.
- [다운로드(.unitypackage)](https://developers.google.com/unity/archive#external_dependency_manager_for_unity)  

구글 로그인 SDK가 설치된 이후, 유니티 상단의 Assets > External Dependency Manager > Android Resolver > Force Resolve를 클릭하여 종속성된 플러그인들을 다운받습니다.

![](/img/docs/guide/toolkit/google-login/use-force-resolve.png)

EDM4U가 정상적으로 실행되었다면 성공했다는 모달이 표시됩니다.  

![](/img/docs/guide/toolkit/google-login/edm4u-success.png)

성공 이후에는 Assets > Plugins > Android 폴더에서 구글 로그인용 플러그인이 생성된 것을 확인하실 수 있습니다.

![](/img/docs/guide/toolkit/google-login/check-android-plugins.png)

### EDM4U Google.IOSResolver.dll 에러
EDM4U 설치 시, 다음과 같은 에러가 발생할 수 있습니다.  
해당 에러는 사용하는 데에는 문제가 없지만, 만약 해결을 원하실 경우 해당 유니티 버전의 iOS 플랫폼을 추가해야합니다.

![](/img/docs/guide/toolkit/google-login/edm4u-ios-error.png)

### EDM4U 사용이 불가할 경우
만약 EDM4U 사용이 불가능하거나, 에러가 지속적으로 발생할 경우, android 플러그인들을 직접 설치해야합니다.  

그러나 직접 설치 시에는 이름은 같으나 버전이 다른 플러그인들의 중복 발생으로 인해 빌드가 실패할 수 있으니, 이름이 같고 버전이 다른 플러그인들은 제거를 해주셔야합니다.  

**Android 추가 플러그인(EDM4U 사용이 불가능한 경우)** : [BackendGoogleLogin-extra-plugins.unitypackage](https://developer.thebackend.io/sdk/google-login/BackendGoogleLogin-extra-plugins.unitypackage) \[2024-02-22]  
:::caution progard user안내
만약 프로가드를 사용중일 경우, 아래와 같은 예외 처리가 필요합니다.  
```
-keep class io.thebackend.googlelogin.** {*;}
```
:::

## 초기 구글 콘솔 설정  
구글로 로그인을 사용하기 위해서는 구글 클라우드 플랫폼(GCP)에서 Client ID를 생성해야 합니다.  
Android와 iOS 두 플랫폼 모두 지원할 경우, 공동의 GCP 프로젝트에서 각각 Adnorid, iOS Client ID를 생성하면 됩니다.  

**GPGS로 로그인 과정 중 GPGS 플러그인 사용 및 구글 콘솔에서 GPGS 설정하는 부분을 제외하고는 동일한 방법으로 진행됩니다.**  


안드로이드 : [안드로이드 추가 콘솔 설정](/sdk-docs/backend/toolkit/google-login/android/android-settings)  
iOS : [iOS 추가 콘솔 설정](/sdk-docs/backend/toolkit/google-login/ios/ios-settings)  
