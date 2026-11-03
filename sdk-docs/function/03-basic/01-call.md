---
sidebar_label: 서버에서 뒤끝펑션이 호출되는 형태
description: "서버에서 뒤끝펑션이 호출되는 형태"
---

# 서버에서 뒤끝펑션이 호출되는 형태

서버는 배포된 뒤끝펑션을 아래 규칙으로 찾아 실행합니다.  

- 파일 이름: `BackendFunction.dll`
- 네임스페이스: `BackendFunction`
- 클래스: `BFunc`
- 실행할 함수: `Function`

```csharp
// 동기
public Stream Function(Stream stream, ILambdaContext context)

// 비동기
public async Task<Stream> Function(Stream stream, ILambdaContext context)
```

`backend new`로 만든 프로젝트는 위 규칙대로 설정되어 있습니다.  
`backend build`와 `backend deploy`는 파일(어셈블리) 이름만 `BackendFunction`으로 고정합니다. 코드의 `namespace BackendFunction` 선언은 바꾸지 않습니다.  
네임스페이스, 클래스 이름, 함수 이름, 매개변수 형태를 바꾸어도 빌드와 배포는 성공하지만, 서버에서 함수를 찾지 못해 실행되지 않습니다.  

`Function` 함수는 뒤끝펑션의 Main 함수로 이해하면 쉽습니다.  
`stream`으로 클라이언트가 보낸 값이 들어오고, 반환한 `Stream`이 클라이언트로 전달됩니다.  

:::caution 뒤끝펑션의 비동기 처리
뒤끝펑션을 비동기로 구현하면 동기 방식보다 응답 속도가 느려질 수 있습니다.  
동기 방식으로 구현하는 것을 권장하며, 비동기 처리가 꼭 필요할 때만 사용해 주세요.  
:::

## 임의의 네임스페이스와 클래스 만들기

프로젝트 안에 다른 네임스페이스와 클래스를 자유롭게 만들고 `Function` 함수 안에서 호출할 수 있습니다.  
`BFunc` 클래스에는 `Function`이라는 이름의 함수를 하나만 두어야 합니다. 같은 이름의 함수가 여러 개면 실행할 함수를 정할 수 없습니다.  
