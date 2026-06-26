---
sidebar_label: 뒤끝베이스로 데이터 송신
---

# 뒤끝 베이스로 데이터 송신

뒤끝펑션에서 진행한 일련의 연산 결과는 뒤끝 베이스로 송신할 수 있습니다.  

서버에 배포된 뒤끝펑션은 BackendFunction.BackendFunction.BFunc.Function(dll명, 네임스페이스 명, 클래스 명, 함수 명) 을 실행합니다.  

**Function 함수의 리턴값은 Stream으로 이 값이 뒤끝 SDK로 송신**되게 됩니다.  

데이터를 Stream으로 변환하는 방법은 아래 개발자 문서를 참고해주세요.  

- [String을 Stream으로 변환](/sdk-docs/function/transfer-stream/string-stream)
- [JsonData를 Stream으로 변환](/sdk-docs/function/transfer-stream/jsondata-stream)
