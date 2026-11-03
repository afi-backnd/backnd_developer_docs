---
sidebar_label: 호출한 사용자 확인하기
description: "호출한 사용자 확인하기"
---

# 호출한 사용자 확인하기

함수를 호출한 사용자의 데이터를 바꾸거나 판매자·구매자를 기록하려면, 호출한 사용자가 누구인지 서버가 확인한 값이 필요합니다.  
**요청 값(`Backend.Content`)으로 받은 사용자 ID는 사용하지 않습니다.** 클라이언트가 다른 사람의 ID를 보내면 그 사람의 데이터를 바꿀 수 있게 되기 때문입니다.  

## gamerId 확인하기

함수 안에서 `Backend.BMember.GetUserInfoV2()`를 호출하면, 함수를 호출한 사용자의 정보를 서버에서 받아옵니다.  
결과의 `row.gamerId`가 호출한 사용자의 gamerId입니다.  

```csharp
// Backend.Initialize(ref stream)를 호출한 뒤에 실행합니다.
var userInfo = Backend.BMember.GetUserInfoV2();
if (userInfo.IsSuccess() == false)
{
    return ReturnErrorObject("fail to get user info: " + userInfo.GetMessage());
}

string gamerId = userInfo.GetReturnValuetoJSON()["row"]["gamerId"].ToString();
```

- 이 값은 유니티가 로그인할 때 받은 토큰으로 서버가 확인한 값이므로, 요청 값을 바꿔도 달라지지 않습니다.  
- 멀티 캐릭터 프로젝트에서는 유니티에서 선택한 캐릭터의 ID가 들어옵니다.  
- 뒤끝 데이터베이스의 `OfCurrentUser()`도 같은 사용자를 대상으로 합니다. [뒤끝 데이터베이스 사용](/sdk-docs/function/backnd-features/database) 문서를 참고해 주세요.  
- 서버에 한 번 더 요청하므로 함수의 응답 시간이 늘어납니다. 한 번 호출할 때 한 번만 확인하고 변수에 담아 사용합니다.  

## 내 PC에서 실행할 때

`GetUserInfoV2`는 로그인한 사용자가 있어야 동작합니다.  

| 실행 방법 | 결과 |
| --- | --- |
| `backend debug` (기본 실행) | 로그인한 사용자가 없어 실패합니다. |
| `backend debug --user`, 편집기의 테스트 계정 실행 | 등록한 테스트 계정의 gamerId를 받습니다. |
| 배포한 뒤 `backend invoke` | 등록한 테스트 계정의 gamerId를 받습니다. |
| 배포한 뒤 유니티에서 호출 | 유니티에서 로그인한 사용자의 gamerId를 받습니다. |

테스트 계정 등록은 [인증 정보 등록](/sdk-docs/function/cli/setting) 문서를 참고해 주세요.  
