---
sidebar_label: 디버깅
description: "디버깅"
---

# 디버깅

함수를 빌드 후 서버에 배포하여 테스트하기 전에 뒤끝 SDK에서 파라미터를 보냈을 때 실행 결과가 어떻게 리턴될지 확인할 수 있습니다.  
뒤끝 SDK에서 뒤끝펑션을 호출하는 방법은 [해당 개발 문서](/sdk-docs/backend/base/function/execute-function)에서 확인할 수 있습니다.  

:::caution 뒤끝펑션 개발 툴
정상적으로 디버깅을 진행하기 위해서는 반드시 뒤끝펑션 개발 툴이 설치되어 있어야 합니다.  
:::

## 디버깅 환경 설정하기

---
####
1. 디버깅을 위해서는 먼저 debugConfig.json을 설정해야 합니다.  

솔루션 탐색기에서 debugConfig.json을 확인합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/debug/1.PNG)

---
####
2. debugConfig.json 파일을 설정합니다.  

### debugConfig.json

```js
{
  "clientAppId": "뒤끝 콘솔에서 발급받은 clientAppId",
  "signatureKey": "뒤끝 콘솔에서 발급받은 signatureKey",
  "packageName": "앱의 패키지 명. 빌드 할 프로그램의 패키지 명과 뒤끝 콘솔에 입력한 패키지 명과 디버깅 설정에 입력한 패키지 명이 모두 일치해야 합니다. ",
  "authKey": "뒤끝 콘솔에서 발급받은 뒤끝펑션 인증키",

  "testId": "디버깅을 실행할 테스트 ID. 커스텀 회원가입 혹은 뒤끝 콘솔에서 생성을 통해 실제로 존재하는 계정의 ID를 입력해야 합니다.",
  "testPw": "테스트 PW. 위와 동일하게 실제로 존재하는 계정의 PW를 입력해야 합니다.",

  "timeoutEnable": "false", // 디버깅 시 타임아웃 활성화 여부(string 문자열입니다. "true = 활성화")

  // 뒤끝펑션으로 전달할 파라미터
  // content 내부의 값이 뒤끝펑션으로 전달됩니다.  
  "content": {
    // json 형태로(key-value 쌍 형태로) 값을 입력해야 합니다.  
    // 뒤끝 SDK에서 Param으로 호출할 값을 json 형태로 설정할 수 있습니다.  
    // 뒤끝펑션 내부에서 아래 값을 Backend.Content 변수를 이용하여 json 형태로 불러올 수 있습니다.  
    "value": "HelloWorld!"
  }
}
```

> 뒤끝펑션 인증키 발급받는 법은 [시작하기 문서](/sdk-docs/function/intro)를 참고해 주세요.  

---
####
3. 빌드 구성을 설정합니다.  

빌드 구성은 **디버그 모드**로, 디버깅 툴은 **BF Debug Tool**로 설정되어 있어야 정상적으로 디버깅을 진행할 수 있습니다.  

![image](https://developer.thebackend.io/static/img/bfunc/debug/2.PNG)

### 디버깅 툴 실행에 실패한 경우 아래 사항을 확인해 주세요.  

- 뒤끝펑션 개발 툴이 설치되어 있는지 확인해 주세요.  
- 디버깅 구성 설정이 **BF Debug Tool**로 설정되어 있는지 확인해 주세요.  
- 프로젝트 속성에 디버그 항목이 아래 그림과 같이 설정되어 있는지 확인해 주세요.  

![image](/img/docs/guide/function/vs/debug-profile.png)

---
####
4. 디버깅을 수행합니다.  

### 디버깅 성공

정상적으로 디버깅 툴 실행에 성공하면 아래 그림과 같이 표시되게 됩니다.  

![image](https://developer.thebackend.io/static/img/bfunc/debug/debug%20success.PNG)

### 디버깅 리턴 값이 이상할 때

**Backend Function Return Value:** 에 표시된 내용이 이상할 때는 비주얼 스튜디오에서 `솔루션 탐색기 > 프로젝트명 우클릭 > 다시 빌드`를 한 후 디버깅을 시도해보세요.  

### 디버깅 실패

debugConfig.json에 잘못된 값이 설정되어 있으면 아래와 같이 에러가 발생하게 됩니다.  
붉은 글씨로 뒤끝펑션 실행에 실패한 이유가 표시됩니다.  

![image](https://developer.thebackend.io/static/img/bfunc/debug/debug%20fail.PNG)

### errorCase

- **debugConfig.json의 값 중 필요한 키가 존재하지 않을 때**  
  Object reference not set to an instance of an object.  

- **debugConfig.json의 값 중 null 혹은 공백인 값이 있을 때**  
  fail to read debugConfig.json: some flag is null or empty
  "debugConfig.json 내용을 출력"

- **Client App ID 가 잘못된 경우**  
  errorCode : BadUnauthroizedException | message : bad clientAppId, 잘못된 clientAppId 입니다.  

- **Signature Key 가 잘못된 경우**  
  errorCode : BadUnauthroizedException | message : bad signatureKey, 잘못된 signatureKey 입니다.  

- **존재하지 않는 계정의 id를 입력한 경우**  
  errorCode : BadUnauthorizedException | message : bad customId, 잘못된 customId 입니다

- **계정의 pw를 잘못 입력한 경우**  
  errorCode : BadUnauthorizedException | message : bad customPassword, 잘못된 customPassword 입니다

- **Fuction함수의 반환 형태가 잘못된 경우**  
 System.ArgumentException: Cannot bind to the target method because its signature is not compatible with that of the delegate type.  

 Fuction함수의 형태는 아래와 같아야합니다.  
 ```js
 public Stream Function(Stream stream, ILambdaContext context)  
 또는 비동기 방식으로 구현했을 경우
 public async Task<Stream> Function(Stream stream, ILambdaContext context)  
 ```
