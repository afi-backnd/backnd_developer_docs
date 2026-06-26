---
sidebar_label: 설정
---

# 설정

개발사에서 생성한 뒤끝펑션 프로젝트를 빌드, 배포, 삭제하기 위해 Backend CLI를 설정하기 위한 방법입니다.  

## config 명령어

Backend CLI를 제어하기 위한 config.json 파일을 메모장을 이용하여 실행합니다.  
config.json 파일은 아래 경로에 저장되어 있습니다.  

> %appdata%/AFI/BFunc/config.json

**config 명령어 실행하기**  
backend config

### config.json

```js
{
  "account": {
    // 뒤끝 콘솔에서 발급받은 뒤끝펑션 인증키
    "authKey": "인증키"
  },
  "projectInfo": {
    // 개발자가 생성한 프로젝트의 절대 경로를 입력합니다.  
    // 경로에는 반드시 \\로 구분해야 CLI에서 정상적으로 인식할 수 있습니다.  
    // ex) C:\\work
    "projectPath": "프로젝트 경로",
    // 개발사에서 생성한 프로젝트의 명을 입력합니다.  
    "csprojName": "BackendFunction.csproj",
    // 서버로 배포할 프로젝트의 결과물입니다. 기본값은 publish.zip입니다. 변경하지 않는 것을 권장합니다.  
    "binName": "publish.zip"
  },
  // 배포/삭제할 함수 명. 여기서 설정한 함수 명으로 서버에 배포됩니다.  
  "functionName": "publish",
  // 함수에 대한 설명. 여기서 설정한 문자열로 서버에 배포됩니다.  
  "description": "요청 보냄"
}
```

> 만약 C:/work/FuncDir/func.csproj 파일을 설정하려면 아래와 같이 설정합니다.  
> **projectPath: "C:\\\work\\\FuncDir"**  
> **csproj: "func.csproj"**  
>   

> 뒤끝펑션 인증키 발급받는 법은 [시작하기 문서](/sdk-docs/function/intro)를 참고해 주세요.  
