# 뒤끝펑션 — Q&A

## Q. 뒤끝펑션이란 무엇인가요?

뒤끝펑션은 개발사가 직접 서버에서 실행할 커스텀 로직을 작성하고 배포할 수 있는 서버리스 함수 실행 환경입니다.
베이스 SDK와 함께 사용하며, 뒤끝펑션 단독으로는 사용할 수 없습니다.

## Q. 어떤 경우에 뒤끝펑션을 사용하나요?

- 클라이언트에서 처리하면 보안에 취약한 로직(확률, 보상 계산 등)
- 여러 SDK 기능을 서버에서 조합하여 원자적으로 실행해야 하는 경우
- 외부 API 호출이 필요한 경우

## Q. 뒤끝펑션을 사용하려면 어떻게 시작하나요?

1. 뒤끝 콘솔에서 뒤끝펑션 메뉴 → 활성화
2. 인증키 발급
3. Unity `TheBackend > Edit Settings`에서 Function Auth Key 입력
4. 뒤끝 콘솔에서 함수 코드 작성 및 배포

자세한 내용은 [뒤끝펑션 시작하기](../sdk-docs/function/)를 참고하세요.

## Q. 클라이언트에서 펑션을 호출하는 방법은?

```csharp
Backend.Function.Call("함수이름", new Param(), callback =>
{
    if (callback.IsSuccess())
    {
        Debug.Log("펑션 호출 성공");
    }
});
```

자세한 내용은 [베이스 SDK 펑션 호출 문서](../sdk-docs/backend-new/02-base/31-function/)를 참고하세요.
