---
sidebar_label: "Unity 설정"
sidebar_position: "0.5"
draft: "true"
description: "Unity 설정"
---

# Unity 설정

:::caution Unity 지원 버전 안내  
SDK 3.0.0+ 에 적용된 안드로이드 인증(credentials) 기능은 AGP(Android Gradle Plugin)8.x버전 이상에서 빌드가 가능합니다.  
Unity6+는 AGP8.x를 공식 지원하여 빌드가 가능하지만 하위 버전은 공식 지원하지 않아 빌드가 되지 않습니다.  

**따라서 하위 버전에서는 별도의 설정이 필요하며 이는 Unity의 공식지원은 아니므로 개발환경에 따라 동작하지 않을 수 있습니다.**  
**아래의 설정대로 진행해도 문제가 발생할 경우, Unity6로 프로젝트를 업데이트 하여 빌드를 진행하시는 것을 권장드립니다.**  

Unity에서 지원하는 AGP 버전은 아래의 링크에서 확인 가능합니다.  
[Unity AGP Version](https://docs.unity3d.com/6000.0/Documentation/Manual/android-gradle-overview.html)
:::  

## Unity6+의 경우
### 1. Android API Level 설정
- Unity 메뉴의 **Edit > Project Settings > Player > Other Settings** 에서 **Target API Level을 35 이상으로 설정**합니다.    

### 2. dependencies 설정
- 아래의 dependencies 를 mainTemplate.gradle 에 추가해야 합니다.
  - implementation 'androidx.credentials:credentials:1.5.0'
  - implementation 'androidx.credentials:credentials-play-services-auth:1.5.0'
  - implementation 'com.google.android.libraries.identity.googleid:googleid:1.1.1'
- EDMU(External Dependency Manager for Unity)가 설치된 경우.
  - Unity 메뉴의 **Asset > External Dependency Manager > Android Resolver > Force Resolve** 를 클릭하면 자동으로 추가됩니다.
- EDMU(External Dependency Manager for Unity)가 없는 경우.
  - Unity 메뉴의 **Edit > Project Settings > Player > Publishing Settings 에서 Custom Main Gradle Template**을 체크합니다.
  - **Assets\Plugins\Android\mainTemplate.gradle** 의 파일을 열어 위의 dependencies를 추가합니다.  
- **mainTemplate.gradle** 설정 예시
```
dependencies {
  ... 

  implementation 'androidx.credentials:credentials:1.5.0'
  implementation 'androidx.credentials:credentials-play-services-auth:1.5.0'
  implementation 'com.google.android.libraries.identity.googleid:googleid:1.1.1'
}
```

Unity6+는 AGP 8.x를 공식지원하므로 위의 내용만 설정해 주시면 됩니다.
    
## Unity 2021, 2022의 경우
### 1. 지원 가능한 Unity 버전 설치
- AGP 버전이 7.4.2로 변경된 버전부터 지원 가능합니다.  
- Unity2022 : **Unity2022.3.38f1** 이상 버전.
- Unity2021 : **Unity2021.3.41f1** 이상 버전. 
- Unity2020을 포함한 그 이하 버전은 지원하지 않습니다.  

### 2. Gradle 라이브러리(8.1.1) 설치
- [gradle-8.1.1 다운로드](https://services.gradle.org/distributions/gradle-8.1.1-bin.zip)
- 다운받은 zip파일을 적당한 위치에 압축을 풉니다.  
- Unity 메뉴의 **Edit > Preferences > External Tools > Gradle Installed with Unity** 항목의 체크를 해제합니다.
- 빈 경로창에 방금 압축을 푼 gradle폴더의 경로를 지정합니다.  
  - ex) C:\libs\gradle\gradle-8.1.1    
![image](/img/docs/guide/toolkit/google-login/googlelogin-android-gradle-settings.png)  

### 3. OpenJDK 17 라이브러리 설치
- [OpenJDK 17 공식 배포 사이트](https://adoptium.net/temurin/releases/?os=windows&arch=x64&package=jdk&version=17)
  - 문서 작성 기준 17.0.15+6을 다운받아서 사용했습니다.
- 다운받은 zip파일을 적당한 위치에 압축을 풉니다.  
- Unity 메뉴의 **Edit > Project Settings > Player > Publishing Settings 에서 Custom Gradle Properties Template** 을 체크합니다.
- **Assets\Plugins\Android\gradleTemplate.properties** 의 파일을 열어 다음의 문구를 아래에 추가합니다.
  - org.gradle.java.home=**[설치경로] (\의 경우, 두 번 입력)**
  - ex) org.gradle.java.home=C:\\\\libs\\\\jdk\\\\jdk-17.0.15+6
- **gradleTemplate.properties** 설정 예시
```
**ADDITIONAL_PROPERTIES**
org.gradle.java.home=C:\\libs\\jdk\\jdk-17.0.15+6
```

### 4. AGP(Android Gradle Plugin) 설정
- Unity 메뉴의 **Edit > Project Settings > Player > Publishing Settings 에서 Custom Base Gradle Template**을 체크합니다.
- **Assets\Plugins\Android\baseProjectTemplate.gradle** 의 파일을 열어 version을 수정합니다.
  - id 'com.android.application' version '7.x.x' 의 version 값을 '8.1.4'로 수정.
  - id 'com.android.library' version '7.x.x' 의 version 값을 '8.1.4'로 수정.
- **baseProjectTemplate.gradle** 설정 예시
```
plugins {
  ... 

  id 'com.android.application' version '8.1.4' apply false
  id 'com.android.library' version '8.1.4' apply false
}
```

### 5. dependencies 설정
- 아래의 dependencies 를 mainTemplate.gradle 에 추가해야 합니다.
  - implementation 'androidx.credentials:credentials:1.5.0'
  - implementation 'androidx.credentials:credentials-play-services-auth:1.5.0'
  - implementation 'com.google.android.libraries.identity.googleid:googleid:1.1.1'
- EDMU(External Dependency Manager for Unity)가 설치된 경우.
  - Unity 메뉴의 **Asset > External Dependency Manager > Android Resolver > Force Resolve** 를 클릭하면 자동으로 추가됩니다.
- EDMU(External Dependency Manager for Unity)가 없는 경우.
  - Unity 메뉴의 **Edit > Project Settings > Player > Publishing Settings 에서 Custom Main Gradle Template**을 체크합니다.
  - **Assets\Plugins\Android\mainTemplate.gradle** 의 파일을 열어 위의 dependencies를 추가합니다.  
- **mainTemplate.gradle** 설정 예시
```
dependencies {
  ... 

  implementation 'androidx.credentials:credentials:1.5.0'
  implementation 'androidx.credentials:credentials-play-services-auth:1.5.0'
  implementation 'com.google.android.libraries.identity.googleid:googleid:1.1.1'
}
```

### 6. Android API Level 설정
- Unity 메뉴의 **Edit > Project Settings > Player > Other ettings** 에서 **Minimum API Level을 24 이상으로 설정**합니다.    
- Unity 메뉴의 **Edit > Project Settings > Player > Other Settings** 에서 **Target API Level을 35 이상으로 설정**합니다.    
- api 레벨 설정 화면    
![image](/img/docs/guide/toolkit/google-login/googlelogin-android-api-settings.png)  
- 빌드 진행시, API Level에 해당하는 SDK가 없으면 Unity에서 설치여부를 물어보는데 Yes를 눌러 설치를 하시면 됩니다.

### 7. (Optional) Android SDK build-tool 설치
Android SDK build-tool 버전이 33.0.1 이상에서만 API Level 35 이상 빌드가 가능합니다.  
만약 사용하시는 Unity에 해당 build-tool 버전이 없다면 Android Studio를 통해 build-tool을 다운받아 설치해야 합니다.  
이미 33.0.1 이상 버전을 사용하시는데도(ex:34.0.0) 빌드가 되지 않는 다면 33.0.1로 바꾸어 시도해 보시기 바랍니다.  

#### 1. Android SDK build-tool 다운로드
- Android Studio를 설치합니다.
  - [Android Studio 공식 배포 사이트](https://developer.android.com/studio)
- 설치 후, New Project로 빈 프로젝트를 하나 생성합니다.
- 프로젝트가 열리면 메뉴에서 **Tools > SDK Manager** 를 선택해서 SDK Manager 창을 엽니다.
- 열린 창에서 **SDK Tools** 탭을 선택하고 **33.0.1** 버전이 설치되어 있는지 확인 합니다.
- 설치되어 있지 않다면 체크 후, Apply를 눌러 설치를 진행합니다.
- 상단에 있는 Android SDK Location 경로로 이동하여 33.0.1을 copy하여 2번 단계에서 사용합니다.  
![image](/img/docs/guide/toolkit/google-login/googlelogin-android-build-tool-settings.png)   

#### 2. 설치 방법
- Unity 에디터가 설치된 폴더에서 SDK가 있는 폴더로 이동합니다.
  - 경로는 Unity 메뉴의 **Edit > Preferences > External Tools > Android SDK Tools Installed with Unity** 에서 copy path를 하면 알 수 있습니다.
- build-tools 폴더로 이동합니다
- 해당 폴더에 1번 단계에서 다운받은 build-tool 폴더(33.0.1)를 복사해서 붙여 넣습니다.
  - 만약 빌드 버전 충돌이 발생할 경우, 기존의 build-tool 폴더(3x.x.x)를 다른 곳으로 이동시킨 후, 다시 빌드를 진행해 보세요.

