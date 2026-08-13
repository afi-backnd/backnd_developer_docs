---
draft: "true"
unlisted: "true"
description: "콜백 함수 풀링"
---

# 콜백 함수 풀링

콜백 함수 풀링의 경우 뒤끝 기능을 비동기로 서버에 요청 후 응답이 왔을 때 콜백 함수를 별도의 큐에 저장해두고,
메인 스레드에서 이를 실행하는 기능입니다.  

이를 통해 **비동기 함수의 콜백 함수 내에서도 유니티의 Monobehavior 객체에 접근할 수 있습니다.**  

## 1. SDK 초기화 시 설정

콜백 함수 풀링을 사용하기 위해서는 뒤끝 SDK를 초기화할 때 **useAsyncPoll 인자를 true로 설정** 해야 합니다.  

### 첫 번째 SDK 초기화 방법
```js
void Start() {
  var bro = Backend.Initialize(); // useAsyncPoll 인자를 true로 설정
  if(bro.IsSuccess()) {
    // 초기화 성공 시 로직
    } else {
      // 초기화 실패 시 로직
    } 
}
```

### 두 번째 SDK 초기화 방법
```js
void Start() {
  Backend.InitializeAsync(true, callback => {
    if(callback.IsSuccess()) {
      // 초기화 성공 시 로직
    } else {
      // 초기화 실패 시 로직
    }    
}); // useAsyncPoll 인자를 true로 설정
}
```

## 2. 풀링 함수 호출

풀링 함수 기능을 사용하도록 설정하면 비동기 함수의 콜백은 비동기 IO 스레드에서 실행되지 않고 별도의 큐에 저장되게 됩니다.  
큐에 저장된 **콜백 함수를 실행하기 위해서는 아래 풀링 함수를 주기적으로 호출**해야 합니다.  


```js
void Update() {
  Backend.AsyncPoll();
}
```

풀링 함수를 주기적으로 호출하기 위해 아래 방법이 권장됩니다.  
* 유니티 객체의 Update 함수 내에 풀링 함수 선언
* 주기적으로 호출되는 코루틴을 생성하고 해당 코루틴 내에서 풀링 함수 선언
* 별도의 주기적으로 호출되는 함수를 생성하고 해당 함수 내에 풀링 함수 선언

풀링 함수를 주기적으로 호출하기 위해 아래 방법이 권장되지 않습니다.  
* 재귀 함수를 선언하고, 재귀 함수 내에 풀링 함수 선언
