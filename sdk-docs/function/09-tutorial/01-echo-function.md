---
sidebar_label: 에코 함수
---

# 에코 함수

뒤끝펑션을 이용하여 **뒤끝 SDK 함수에서 요청한 문자열을 그대로 리턴**하는 뒤끝펑션을 만들고,
뒤끝펑션 개발 툴을 이용하여 **디버깅, 빌드, 배포하는 방법에 대한 튜토리얼**입니다.  
튜토리얼을 정상적으로 진행하기 위해서는 뒤끝펑션 개발 툴과 뒤끝펑션 프로젝트 템플릿이 설치되어 있어야 합니다.  

---
####
1. BackendFunction 템플릿을 이용하여 새 프로젝트를 생성합니다.  

[프로젝트 생성 문서](/sdk-docs/function/work-in-windows/visual-studio-code/create)를 참고하여 새 프로젝트를 생성합니다.  

---
####
2. Function 함수를 아래와 같이 작성합니다.  

```js
public Stream Function(Stream stream, ILambdaContext context)
{
    try
    {
        // 뒤끝펑션 API 초기화
        Backend.Initialize(ref stream);
    }
    catch(Exception e)
    {
        // 뒤끝펑션 API 초기화를 실패한 경우
        return ReturnErrorObject("initialize " + e.ToString());
    }

    // debugConfig.json에서 `content` 혹은 뒤끝 SDK에서 InvokeFunction을 호출할 때 인자 값으로 넘긴 `Param` 에
    // value 키가 존재하는지 확인합니다.  
    if(Backend.HasKey("value") == false)
    {
      return ReturnErrorObject("value key is not exist");
    }

    // debugConfig.json에서 `content` 혹은 뒤끝 SDK에서 InvokeFunction을 호출할 때 인자 값으로 넘긴 `Param` 은
    // Backend.Content를 이용하여 엑세스할 수 있습니다.  
    return Backend.StringToStream(Backend.Content["value"].ToString());
}
```

---
####
3. debugConfig.json 파일을 설정합니다.  

[디버깅 문서](/sdk-docs/function/work-in-windows/visual-studio-code/debug)를 참고하여 디버깅 설정을 합니다.  
이때 debugConfig.json에서 content는 아래와 같이 설정합니다.  

### debugConfig.json

```js
{
  ...  
  "content": {
    "value": "HelloWorld!" // 원하는 문자열을 HelloWorld! 대신 입력합니다.  
  }
}
```

---
####
4. 디버깅을 진행합니다.  

아래 그림과 같이 결과가 나오면 정상적으로 진행된 것입니다.  

![image](https://developer.thebackend.io/static/img/bfunc/tutorial/debug.PNG)

---
####
5. Backend CLI config.json을 설정합니다.  

cmd에서 `backend config`를 입력하고 열린 메모장에서 authKey를 제외하고 아래와 같이 입력 후 저장을 합니다.  

```js
{
  "account": {
    "authKey": "뒤끝 콘솔에서 발급받은 뒤끝펑션 인증키"
  },
  "projectInfo": {
    // 개발자가 생성한 프로젝트의 절대 경로를 입력합니다.  
    // 경로에는 반드시 \\로 구분해야 CLI에서 정상적으로 인식할 수 있습니다.  
    // ex) C:\\work\\funcSet
    "projectPath": "프로젝트 경로",
    // 개발사에서 생성한 프로젝트의 명을 입력합니다.  
    "csprojName": "BackendFunction.csproj",
    "binName": "publish.zip"
  },
  "functionName": "echoFunction",
  "description": "에코 기능을 수행하는 함수입니다."
}
```

---
####
6. Backend CLI를 이용하여 해당 프로젝트를 빌드합니다.  

cmd에서 `backend build` 명령어를 입력합니다.  
[빌드 문서](/sdk-docs/function/cli/build)를 참고해 주세요.  

---
####
7. Backend CLI를 이용하여 빌드 결과물을 서버로 배포합니다.  

cmd에서 `backend deploy` 명령어를 입력합니다.  
[배포 문서](/sdk-docs/function/cli/deploy)를 참고해 주세요.  

---
####
8. 뒤끝콘솔에서 정상적으로 배포가 되었는지 확인합니다.  

![image](https://developer.thebackend.io/static/img/bfunc/tutorial/echo.png)

---
####
9. 유니티에서 뒤끝 SDK 함수를 호출하여 배포한 함수가 정상적으로 리턴되는지 확인합니다.  

뒤끝 SDK에서 아래와 같이 함수를 호출해보고, 정상적으로 리턴되는지 확인합니다.  

```js
Param param = new Param();
param.Add("value", "HelloWorld!");

var bro = Backend.BFunc.InvokeFunction("echoFunction", param);
```

**성공하면 bro가 아래와 같이 리턴됩니다.**  
statusCode : 200  
message : Success  
returnValue : {"result":"HelloWorld!"}
