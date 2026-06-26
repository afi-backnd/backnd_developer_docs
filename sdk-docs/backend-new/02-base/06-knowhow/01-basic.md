# 뒤끝 사용하기 기초

## 네임스페이스 선언

뒤끝 SDK를 사용하기 위해서는 **뒤끝 기능을 호출하는 모든 스크립트에서 Using 키워드를 사용하여 BACKND.Base 네임스페이스를 사용하겠다고 선언**해야 합니다.  

### Example
```csharp
using BACKND.Base;
```

## 뒤끝 SDK 초기화

뒤끝 SDK에 포함되어 있는 모든 클래스들은 뒤끝 SDK를 초기화할 때 함께 초기화됩니다.  
뒤끝 SDK 초기화를 위해서는 [SDK 초기화 문서](/sdk-docs/backend/base/sdk-initialize)를 참고해 주세요.  

## 뒤끝 기능 호출

뒤끝의 기능은 **각 기능별 싱글톤 객체** 를 통해 호출할 수 있습니다.  
그 외에 저장 정보(ex: 유저InDate, 유저닉네임등)나 초기화는 **BackndBase**를 통해 접근 가능합니다.  
각 기능을 호출하는 자세한 설명은 각 기능의 문서를 참고해 주세요.  

> 각 기능 객체의 경우, **Backnd**를 시작으로 뒤에 기능이름이 붙습니다. 인증 및 유저정보의 경우, **BackndAuth**가 됩니다.  

### Example
```csharp
using BACKND.Base;

async void MyFunction() {
    var bro = await BackndAuth.Instance.GetUserInfoAsync();
    Debug.Log(bro);
}
```
