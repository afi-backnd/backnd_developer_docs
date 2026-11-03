---
sidebar_label: "[Deprecated] 뒤끝베이스로 데이터 송신"
description: "뒤끝베이스로 데이터 송신"
draft: true
unlisted: true
---

# 뒤끝 베이스로 데이터 송신

뒤끝펑션에서 진행한 일련의 연산 결과는 뒤끝 베이스로 송신할 수 있습니다.  

서버에 배포된 뒤끝펑션은 `BackendFunction.dll`의 `BackendFunction.BFunc.Function`(네임스페이스 `BackendFunction`, 클래스 `BFunc`, 함수 `Function`)을 실행합니다. 자세한 내용은 [서버에서 뒤끝펑션이 호출되는 형태](/sdk-docs/function/basic/call) 문서를 참고해 주세요.  

**Function 함수의 리턴값은 Stream으로 이 값이 뒤끝 SDK로 송신**되게 됩니다.  

데이터를 Stream으로 변환하는 방법은 아래 개발자 문서를 참고해주세요.  

- [String을 Stream으로 변환](/sdk-docs/function/transfer-stream/string-stream)
- [JsonData를 Stream으로 변환](/sdk-docs/function/transfer-stream/jsondata-stream)
