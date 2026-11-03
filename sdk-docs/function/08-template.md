---
sidebar_label: 뒤끝펑션 템플릿
description: "뒤끝펑션 템플릿"
---

# 뒤끝펑션 템플릿

`backend new`가 만드는 `Function.cs`의 기본 코드입니다.  
두 템플릿 모두 뒤끝 SDK를 초기화한 뒤 함수 로직을 실행하는 구조이며, `--template` 옵션으로 고를 수 있습니다.  

## hello (기본)

클라이언트가 보낸 값을 인사 메시지와 함께 돌려줍니다. 프로젝트가 정상적으로 실행되는지 확인할 때 사용합니다.  

```csharp
using System;
using System.IO;
using Amazon.Lambda.Core;
using BackEnd;
using BackEnd.BackndLitJson;
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
                // 클라이언트가 보낸 값은 이후 Backend.Content로 읽을 수 있습니다.
                Backend.Initialize(ref stream);
            }
            catch (Exception e)
            {
                // 초기화에 실패하면 이유를 반환합니다.
                return ReturnErrorObject("initialize " + e.Message);
            }

            // 여기에 함수 로직을 작성합니다.
            // 예제: 클라이언트가 보낸 값을 인사말과 함께 그대로 돌려줍니다.
            JsonData result = new JsonData();
            result["message"] = "Hello from Backend Function";
            result["input"] = Backend.Content;

            // Stream으로 반환한 값이 함수를 호출한 쪽(유니티의 Backend.BFunc.InvokeFunction)으로 전달됩니다.
            return Backend.JsonToStream(result);
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

`requests/default.json`의 기본 내용(`{"message":"hello"}`)으로 실행하면 아래 값을 반환합니다.  

```json
{"message":"Hello from Backend Function","input":{"message":"hello"}}
```

- `Backend.Initialize(ref stream)`는 함수가 시작할 때 한 번 호출합니다. 호출한 뒤에는 `stream`을 직접 읽지 않습니다.  
- 클라이언트가 보낸 값 읽기: [요청 값 읽기](/sdk-docs/function/request-response/read-input)
- 결과 반환하기: [결과 반환하기](/sdk-docs/function/request-response/return-result)
- 함수 안에서 호출할 수 있는 뒤끝 기능: [뒤끝 기능 호출](/sdk-docs/function/backnd-features/call-features)

:::caution 이전 문서의 using 문
이전 문서의 템플릿에 있던 `using BackendAPI;`, `using BackendAPI.Value;`는 현재 SDK에서 빌드 오류가 납니다.  
`using BackEnd;`를 사용해 주세요.  
:::

## empty

함수 로직 없이 빈 JSON 객체 `{}`를 반환합니다. 함수를 처음부터 작성할 때 사용합니다.  

```shell
backend new MyFunction --template empty
```

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
                // 클라이언트가 보낸 값은 이후 Backend.Content로 읽을 수 있습니다.
                Backend.Initialize(ref stream);
            }
            catch (Exception e)
            {
                // 초기화에 실패하면 이유를 반환합니다.
                return ReturnErrorObject("initialize " + e.Message);
            }

            // 여기에 함수 로직을 작성합니다.

            // Stream으로 반환한 값이 함수를 호출한 쪽(유니티의 Backend.BFunc.InvokeFunction)으로 전달됩니다.
            return Backend.StringToStream("{}");
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

## 비동기 함수로 작성하기

함수 안에서 `await`가 필요하면 아래와 같이 `async Task<Stream>`으로 작성합니다.  
비동기 함수는 동기 함수보다 응답이 느려질 수 있으므로 꼭 필요할 때만 사용해 주세요.  

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Amazon.Lambda.Core;
using BackEnd;
using Newtonsoft.Json.Linq;

[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.Json.JsonSerializer))]

namespace BackendFunction
{
    public class BFunc
    {
        public async Task<Stream> Function(Stream stream, ILambdaContext context)
        {
            try
            {
                Backend.Initialize(ref stream);
            }
            catch (Exception e)
            {
                return ReturnErrorObject("initialize " + e.Message);
            }

            // 시간이 걸리는 계산을 별도 작업으로 실행하고 결과를 기다립니다.
            string result = await Task.Run(() => "BackendFunction");

            return Backend.StringToStream(result);
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

함수 안에서 호출하는 뒤끝 기능은 비동기 함수에서도 동기 방식으로 호출해야 합니다. 뒤끝 데이터베이스만 `await`로 호출합니다. [뒤끝 기능 호출](/sdk-docs/function/backnd-features/call-features) 문서를 참고해 주세요.  
