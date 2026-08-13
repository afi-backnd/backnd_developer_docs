---
description: "뒤끝 사용하기 기초"
---

# 뒤끝 사용하기 기초

## 네임스페이스 선언

뒤끝 SDK를 사용하기 위해서는 **뒤끝 기능을 호출하는 모든 스크립트에서 Using 키워드를 사용하여 BackEnd 네임스페이스를 사용하겠다고 선언**해야 합니다.  

### Example
```csharp
using BackEnd;
```

## 뒤끝 SDK 초기화

뒤끝 SDK에 포함되어 있는 모든 클래스들은 뒤끝 SDK를 초기화할 때 함께 초기화됩니다.  
뒤끝 SDK 초기화를 위해서는 [SDK 초기화 문서](/sdk-docs/backend/base/sdk-initialize)를 참고해 주세요.  

## 뒤끝 기능 호출

뒤끝의 모든 기능은 **Backend** 싱글톤 객체를 통해 호출할 수 있습니다.  
각 기능을 호출하는 자세한 설명은 각 기능의 문서를 참고해 주세요.  

> 네임스페이스의 경우 BackEnd이고, 뒤끝 객체의 경우 Backend입니다. 대소문자 구별을 유의해 주세요.  

### Example
```csharp
using BackEnd;

void MyFunction() {
    var bro = Backend.BMember.GetUserInfo();
    Debug.Log(bro);
}
```
