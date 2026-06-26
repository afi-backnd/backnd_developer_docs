# Proguard

## Proguard 사용 시 SDK 초기화 실패 문제 해결

유니티에서 제공하는 Proguard를 사용 시 SDK 초기화 시 **Exception: Fail To Check OS Setting** 예외 메시지와 함께 SDK 초기화가 실패할 수 있습니다.  

Proguard 사용 시 아래 뒤끝 SDK의 패키지 명을 Proguard의 예외 리스트에 예외 처리 등록해야 합니다.  

```js
-keep class io.thebackend.unity.** {
    *;
}
```

만약 뒤끝 1대1 문의 플러그인을 사용하고 있을 경우 해당 예외 리스트도 추가적으로 작성해야합니다.  
```js
-keep class io.thebackend.unity.** {
    *;
}
-keep class io.thebackend.webview.** {
    *;
}
```