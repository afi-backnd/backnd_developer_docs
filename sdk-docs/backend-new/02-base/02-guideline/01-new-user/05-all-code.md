---
description: "전체 코드"
---

# 전체 코드



## BackendLogin.cs

```js
using UnityEngine;
using System.Threading.Tasks;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class BackendLogin
{
    private static BackendLogin _instance = null;

    public static BackendLogin Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendLogin();
            }

            return _instance;
        }
    }

    public async Task CustomSignUp(string id, string pw)
    {
        Debug.Log("회원가입을 요청합니다.");

        var reqResult = await BackndAuth.Instance.SignUpCustomAsync(id, pw);
        if (reqResult.IsSuccess())
        {
            Debug.Log("회원가입에 성공했습니다. : " + reqResult);
        }
        else
        {
            Debug.LogError("회원가입에 실패했습니다. : " + reqResult);
        }
    }

    public async Task CustomSignIn(string id, string pw)
    {
        Debug.Log("로그인을 요청합니다.");

        var reqResult = await BackndAuth.Instance.SignInCustomAsync(id, pw);
        if (reqResult.IsSuccess())
        {
            Debug.Log("로그인이 성공했습니다. : " + reqResult);
        }
        else
        {
            Debug.LogError("로그인이 실패했습니다. : " + reqResult);
        }
    }

    public async Task UpdateNickname(string nickname)
    {
        Debug.Log("닉네임 변경을 요청합니다.");

        var reqResult = await BackndAuth.Instance.UpdateNicknameAsync(nickname);
        if (reqResult.IsSuccess())
        {
            Debug.Log("닉네임 변경에 성공했습니다 : " + reqResult);
        }
        else
        {
            Debug.LogError("닉네임 변경에 실패했습니다 : " + reqResult);
        }
    }
}
```

## BackendManager.cs

```js
using UnityEngine;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class BackendManager : MonoBehaviour
{
    async void Start()
    {
        // 뒤끝 초기화
        var reqResult = await BackndBase.InitializeAsync();
        // 뒤끝 초기화에 대한 응답값
        if (reqResult.IsSuccess())
        {
            Debug.Log("초기화 성공 : " + reqResult); // 성공일 경우 statusCode 204 Success
        }
        else
        {
            Debug.LogError("초기화 실패 : " + reqResult); // 실패일 경우 statusCode 400대 에러 발생
        }

        Test();
    }

    // 동기 함수를 비동기에서 호출하게 해주는 함수(유니티 UI 접근 불가)
    async void Test()
    {
        await BackendLogin.Instance.CustomSignUp("user1", "1234"); // [추가] 뒤끝 회원가입(주석 처리)
        await BackendLogin.Instance.CustomSignIn("user1", "1234"); // [추가] 뒤끝 로그인
        await BackendLogin.Instance.UpdateNickname("원하는 이름"); // [추가] 닉네임 변겅
        Debug.Log("테스트를 종료합니다.");
    }
}
```

