---
sidebar_label: 서버에서 뒤끝펑션이 호출되는 형태
---

# 서버에서 뒤끝펑션이 호출되는 형태

서버에서는 배포된 뒤끝펑션을 아래 규칙을 기준으로 실행시킵니다.  

- 파일 명 : BackendFunction.dll
- 네임스페이스 명 : BackendFunction
- 클래스 명 : Bfunc
- 실행 시킬 함수 
```js
  //동기
  public Stream Function(Stream stream, ILambdaContext context)    
  //비동기
  public async Task<Stream> Function(Stream stream, ILambdaContext context)
```

:::caution 펑션 비동기 처리 이슈
펑션을 비동기로 구현하여 처리 할 경우 동기 방식 보다 응답 속도가 떨어지는 현상이 발생 할 수 있습니다.  
우선적으로 동기 방식으로 구현하는 것을 권장드리며 비동기 처리가 필요한 상황에 따라서 선택적으로 사용해 주세요.
:::

위 규칙은 뒤끝에서 제공하는 Visual Studio 및 Visual Studio Code 플러그인을 이용하여 프로젝트를 생성했다면 자동으로 설정되게 됩니다.  

프로젝트 생성 후 위 4가지를 임의로 수정 시 뒤끝펑션 배포 후 정상적으로 실행되지 않을 수 있습니다.  

> Function 함수를 뒤끝펑션에서의 Main 함수로 이해하시면 더 쉽게 이해하실 수 있습니다.  
> 동기 / 비동기 구현 방식을 구현 방향에 따라 선택하여 사용 할 수 있습니다.  

## 임의의 네임스페이스/클래스 생성 및 호출

임의의 네임스페이스/클래스는 프로젝트 내에서 얼마든지 생성할 수 있고, 이를 Function 함수 내에서 얼마든지 호출할 수 있습니다.  
