---
sidebar_label: 에코 함수
description: "에코 함수"
---

# 에코 함수

유니티에서 보낸 문자열을 그대로 돌려주는 뒤끝펑션을 만들어 내 PC에서 실행하고, 서버에 배포한 뒤 유니티에서 호출하는 튜토리얼입니다.  

## 준비 사항

- `backend`가 설치되어 있어야 합니다. [Windows](/sdk-docs/function/work-in-windows/install) / [Mac](/sdk-docs/function/work-in-ios/install)
- 뒤끝 콘솔에서 뒤끝펑션을 활성화하고 인증키를 발급받아야 합니다. [시작하기](/sdk-docs/function/intro#뒤끝펑션-인증키-발급)
- 유니티 프로젝트에 뒤끝 SDK가 설정되어 있고, Function Auth Key에 인증키가 입력되어 있어야 합니다.

아래 명령은 Windows PowerShell 기준입니다. Mac에서는 터미널에서 같은 명령을 사용하고, 경로만 Mac 형식으로 바꿉니다.  

## 1. 인증키 등록하기

이미 등록했다면 건너뜁니다.  
뒤끝 콘솔에서 뒤끝펑션 인증키를 복사한 뒤 아래 명령을 실행합니다. Mac에서도 같은 명령입니다.  

```powershell
backend config
```

`Function auth key` 질문에 복사한 인증키를 붙여 넣고 Enter를 누릅니다. 붙여 넣은 값은 화면에 표시되지 않습니다.  
나머지 질문은 Enter를 눌러 넘어갑니다. 이 튜토리얼은 테스트 계정이 필요하지 않습니다.  
아래와 같이 `deploy: configured`가 표시되면 등록된 것입니다. 질문의 자세한 내용은 [인증 정보 등록](/sdk-docs/function/cli/setting)을 참고해 주세요.  

```text
Saved profile 'default'.
  deploy:       configured
  SDK identity: not configured
  test user:    none
```

## 2. 프로젝트 만들기

```powershell
mkdir C:\work -Force
cd C:\work
backend new echoFunction
cd echoFunction
```

프로젝트 이름 `echoFunction`이 서버에 배포될 함수 이름이 됩니다.  
편집기에서 프로젝트를 여는 방법은 Windows: [Visual Studio Code](/sdk-docs/function/work-in-windows/visual-studio-code/create) / [Visual Studio](/sdk-docs/function/work-in-windows/visual-studio/create), Mac: [Visual Studio Code](/sdk-docs/function/work-in-ios/visual-studio-code/create) 문서를 참고해 주세요.  

## 3. 함수 작성하기

`Function.cs`의 내용을 모두 지우고 아래 코드로 바꿉니다.  

```csharp
using System;
using System.IO;
using Amazon.Lambda.Core;
using BackEnd;
using Newtonsoft.Json.Linq;

[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.Json.JsonSerializer))]

namespace BackendFunction
{
    public class BFunc
    {
        public Stream Function(Stream stream, ILambdaContext context)
        {
            try
            {
                // 뒤끝펑션 SDK를 초기화합니다.
                Backend.Initialize(ref stream);
            }
            catch (Exception e)
            {
                return ReturnErrorObject("initialize " + e.Message);
            }

            // requests/default.json의 내용 또는 유니티에서 InvokeFunction으로 보낸 Param에
            // value 키가 있는지 확인합니다.
            if (Backend.HasKey("value") == false)
            {
                return ReturnErrorObject("value key is not exist");
            }

            // 받은 value 값을 그대로 돌려줍니다.
            return Backend.StringToStream(Backend.Content["value"].ToString());
        }

        static Stream ReturnErrorObject(string err)
        {
            JObject error = new JObject();
            error.Add("error", err);

            return Backend.StringToStream(error.ToString());
        }
    }
}
```

## 4. 함수에 전달할 값 정하기

프로젝트의 `requests/default.json` 내용을 아래와 같이 바꿉니다.  
이 파일의 내용이 함수에 전달됩니다. 유니티에서 `InvokeFunction`으로 보내는 `Param`과 같은 형태입니다.  
이 함수는 뒤끝 기능을 호출하지 않으므로 인증 정보 없이 실행합니다(`backend debug`의 기본 실행).  

```json
{
  "value": "HelloWorld!"
}
```

## 5. 내 PC에서 실행하기

```powershell
backend debug
```

결과의 `execution`이 아래와 같으면 정상입니다.  

```json
"execution": {
  "error": null,
  "ok": true,
  "result": "HelloWorld!"
}
```

`requests/default.json`에서 `value`를 지우고(`{}`) 다시 실행하면 `result`에 오류 메시지가 표시됩니다. 확인한 뒤에는 `value`를 다시 넣어 주세요.  

```json
"execution": {
  "error": null,
  "ok": true,
  "result": {
    "error": "value key is not exist"
  }
}
```

편집기에서 중단점을 걸어 실행하려면 [Visual Studio Code 디버깅](/sdk-docs/function/work-in-windows/visual-studio-code/debug) / [Visual Studio 디버깅](/sdk-docs/function/work-in-windows/visual-studio/debug) 문서를 참고해 주세요.  

## 6. 서버에 배포하기

```powershell
backend deploy
```

확인 질문 바로 위에 `Deploy function 'echoFunction' with profile '...'` 안내가 표시됩니다. 인증키를 등록한 프로필이 맞는지 확인하고 `y`를 입력합니다.  
결과에 `"ok": true`가 표시되면 배포가 완료된 것입니다.  

## 7. 뒤끝 콘솔에서 확인하기

뒤끝 콘솔의 뒤끝펑션 메뉴에서 `echoFunction`이 배포되었는지 확인합니다.  

## 8. 유니티에서 호출하기

유니티에서 뒤끝 SDK 초기화와 로그인을 마친 뒤 아래 코드를 실행합니다.  

```csharp
Param param = new Param();
param.Add("value", "HelloWorld!");

var bro = Backend.BFunc.InvokeFunction("echoFunction", param);
Debug.Log(bro);
```

성공하면 `bro`가 아래와 같이 반환됩니다.  

```text
StatusCode : 200
Message : Success
ReturnValue : {"result":"HelloWorld!"}
```

`InvokeFunction` 사용법은 [뒤끝펑션 호출](/sdk-docs/backend/base/function/execute-function) 문서를 참고해 주세요.  
