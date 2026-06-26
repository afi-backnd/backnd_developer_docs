# 유니티 플레이어 세팅 설정

뒤끝 SDK를 정상적으로 사용하기 위해서는 Scripting Runtime Version을 **.NET 4 framework**으로 설정해야 합니다.  

## 유니티 버전
유니티 버전이 아래 버전 이상일 때 뒤끝 기능을 정상적으로 사용할 수 있습니다.  
* Unity 2018.2 이상 버전
> Unity 2017.1 이상 버전에서도 가능하지만, 2018.2 이상 버전이 권장됩니다.  

## 플레이어 세팅
`File > Build Settings > Player Settings` 혹은 `Edit > Project Settings > Player`에서 플레이어 세팅을 수정할 수 있습니다.  

플레이어 세팅에서 `Other Settings > Configuration`의 설정이 아래와 같이 설정되어야 합니다.  
이때 Standalone, iOS, Android 이 모두 설정되어야 합니다.  

| 설정 | 값 |
|---|---|
|Scripting Backend | IL2CPP |
| API Compatibility Level | .NET 4.x |
| Scripting Runtime Version  (유니티 최신 버전에서는 존재하지 않을 수 있습니다.) | .NET 4.x Equivalent |
