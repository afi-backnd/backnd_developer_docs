---
sidebar_label: 뒤끝펑션 템플릿
description: "뒤끝펑션 템플릿"
---

# 뒤끝펑션 템플릿

뒤끝펑션의 기본 템플릿에 대한 설명입니다.  
뒤끝펑션 템플릿을 이용하여 프로젝트를 생성하면 초기설정을 완료하면 `Function.cs`에서 다음과 같은 코드를 확인할 수 있습니다.  

**TODO 부분부터 개발사에서 원하는 로직을 작성하고, 그 결과물을 Stream 형태로 리턴이 가능합니다.**  

:::tip 펑션 비동기 처리
펑션의 함수를 아래와 같이 선언하면 await와 같은 비동기 처리가 가능합니다.
```js
 public async Task<Stream> Function(Stream stream, ILambdaContext context)
```
:::

:::caution 펑션 비동기 처리 이슈
펑션을 비동기로 구현하여 처리 할 경우 동기 방식 보다 응답 속도가 떨어지는 현상이 발생 할 수 있습니다.  
우선적으로 동기 방식으로 구현하는 것을 권장드리며 비동기 처리가 필요한 상황에 따라서 선택적으로 사용해 주세요.
:::

```js
using System;
using System.IO;
using System.Text;
using System.Collections.Generic;
using System.Diagnostics;
using System.Threading;

using Amazon.Lambda.Core;
using Newtonsoft.Json.Linq;
using LitJson;

using BackendAPI;
using BackendAPI.Value;

[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.Json.JsonSerializer))]

namespace BackendFunction
{
    public class BFunc
    {
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

            // TODO: 여기에 개발자가 원하는 로직을 작성하면 됩니다.  

            // 원하는 값을 Stream 형태로 리턴하면 해당 값이 뒤끝 SDK로 송신됩니다.  
            return Backend.StringToStream("BackendFunction");
        }

        static Stream ReturnErrorObject(string err)
        {
            JObject error = new JObject();
            error.Add("error", err);

            return Backend.JsonToStream(error.ToString());
        }
    }
}
```
