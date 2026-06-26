# [Deprecated]  SendQueue 사용법

:::warning 신규 버전(6.0.1이상) SDK에서는 SendQueue를 지원하지 않습니다.
신규 버전(6.0.1이상) SDK에서는 더 이상 콜백을 위한 SendQueue를 지원하지 않습니다.  
async/await 로 동작하는 비동기 메서드가 추가되어 더 쉽게 SendQueue와 동일한 동작 구현이 가능합니다.  
async/await 비동기 메서드를 사용해 주시기 바랍니다.
:::

SendQueue를 사용하기 위해 SendQueue 및 스레드를 초기화, 중지, 재시작 등을 하는 사용법입니다.  

:::info SendQueueMgr
아래 함수들을 이용하여 직접 SendQueue 로직을 작성하지 않아도 **SendQueueMgr을 사용하면 SendQueue의 초기화, 중지, 재시작 등의 모든 로직을 손쉽게 게임에 추가할 수 있습니다.**  
<a href="/sdk-docs/backend/toolkit/send-queue-mgr/info" target="_blank">SendQueueMgr 개발자 문서</a>를 확인해보세요.  
:::

## SendQueue 초기화 여부 확인

SendQueue 초기화 여부를 확인하는 변수입니다.  
```js
IsInitialize -> static bool

// example
if(SendQueue.IsInitialize == false) {
  // SendQueue 초기화
}
```

## SendQueue 초기화/시작

SendQueue를 초기화하고 SendQueue가 동작할 스레드를 생성함과 동시에 시작하는 함수입니다.  
* StartSendQueue 함수를 호출하여 **SendQueue를 시작한 경우 별도의 함수를 호출하지 않아도 스레드에서 SendQueue에 적재된 함수를 하나씩 꺼내서 실행시킵니다.**
* 이미 SendQueue가 존재하는데 해당 함수를 호출한 경우 이미 존재하는 SendQueue와 스레드를 제거하고 새로운 SendQueue와 스레드를 생성합니다.  
* 예외 핸들러를 등록하는 경우 SendQueue 내부에서 예외가 발생하였을 때 해당 대리자가 호출됩니다.  


```js
public delegate void ExceptionEvent(Exception e);
StartSendQueue(bool isEnableLog = true, ExceptionEvent exceptionEvent = null) -> static void

// example
if(SendQueue.IsInitialize == false) {
  // SendQueue 초기화
  SendQueue.StartSendQueue(true, ExceptionHandler);
}

void ExceptionHandler(Exception e) {
  // 예외 처리
}
```

### 파라미터
| Value        | Type           | Description  |
| :------------ |:-------------| :----- |
| isEnableLog      | bool | 큐 동작 중 발생하는 예외와 로그를 표시할지 여부 <br />Debug.Log, Debug.LogWarning을 이용하여 로그를 표시합니다. <br />특별한 경우가 아니라면 **true**로 선언하는 것을 추천드립니다. | 
| exceptionEvent      | ExceptionEvent | SendQueue 내부에서 예외가 발생하는 경우 예외를 핸들링 하기 위한 핸들러입니다. | 

## SendQueue 중지

SendQueue가 동작하는 스레드를 중지시키고 삭제합니다.  
현재 SendQueue에 객체가 남아있는지를 확인하지 말고 즉각적으로 Abort 시킵니다.  

* SendQueue가 동작하는 스레드는 Background 스레드로 메인 스레드가 소멸되면 함께 소멸하게 되기 때문에 StopQueue 함수를 호출하지 않아도 스레드가 자동적으로 소멸될 수 있습니다.  
* 하지만 비정상적인 상황에서 게임 종료 시 SendQueue 스레드가 삭제되지 않는 상황이 발생할 수 있기 때문에 **게임 종료 시 StopSendQueue() 함수를 호출하는 것을 권장 드립니다.**  
* 예외 핸들러를 등록하는 경우 SendQueue를 중지했을 때 Abort 예외가 발생하면서 예외 핸들러도 함께 호출됩니다.  
> ThreadAbortException 은 스레드가 종료될 때 발생하는 예외로 무시하셔도 괜찮습니다.  
해당 현상은 추후 픽스 될 예정입니다.  

:::danger 에디터 환경에서는 반드시 StopSendQueue 함수를 호출해야 합니다.  
* 에디터 환경에서는 메인 스레드가 삭제되지 않기 때문에 플레이 버튼을 해제해도 SendQueue 스레드가 삭제되지 않습니다.  
* 반드시 OnApplicationQuit() 함수 등에서 SendQueue 함수를 호출해야 정상적으로 테스트를 진행하실 수 있습니다.  
:::


```js
StopSendQueue() -> static void

// example
void OnApplicationQuit() {
    // 큐에 처리되지 않는 요청이 남아있는 경우 대기하고 싶은 경우
    // 큐에 몇 개의 함수가 남아있는지 체크
    while(SendQueue.UnprocessedFuncCount > 0) {
        // 처리
    }
    
    SendQueue.StopSendQueue();
}

```

## SendQueue 일시정지

SendQueue 동작을 일시정지합니다.  
게임이 백그라운드로 갔을 때 혹은 요청을 멈추고 싶을 때 호출을 권장합니다.  

```js
PauseSendQueue() -> static void

// example
void OnApplicatoinPause(bool isPause) {
  if(isPause == true) {
    // 게임이 Pause 되었을 때
    SendQueue.PauseSendQueue();
  }
}
```

## SendQueue 다시 실행

일시정지된 SendQueue의 동작을 다시 재개시킵니다.  
게임이 백그라운드에서 다시 포그라운드로 돌아왔을 때 호출이 권장됩니다.  

```js
ResumeSendQueue() -> static void

// example
void OnApplicatoinPause(bool isPause) {
  if(isPause == false) {
    // 게임이 다시 진행되었을 때
    SendQueue.ResumeSendQueue();
  }
}
```

## 함수의 콜백으로 선언한 람다 함수 실행

유니티 정책에 따라 메인 스레드가 아닌 별도의 스레드에서는 유니티의 MonoBehavior 객체에 접근할 수 없습니다.  
요청에 대한 응답을 받으면 콜백 함수 큐에 콜백 함수를 삽입하고, Poll 함수에서 이를 실행시킵니다.  
Poll 함수에서는 현재 시점에 콜백 함수 큐에 존재하는 모든 콜백 함수를 꺼내 실행시킵니다.  

```js
Poll() -> static void

// example
// 유니티 객체의 Update 함수나 혹은 정기적(1초 이내)로 호출되는 코루틴 등에서 
// Poll 함수를 호출해 주세요.  
void Update() {
    SendQueue.Poll();
}
```

## 함수의 콜백으로 선언한 람다 함수 하나 실행

아래 함수는 위 Poll 함수와 동일한 기능을 수행하지만,
현재 시점에 콜백 함수 큐에 존재하는 모든 콜백 함수를 실행하는 것이 아닌 하나의 콜백 함수만 꺼내 실행합니다.  
Poll 혹은 PollOneResponse  둘 중 하나의 함수만 호출되어야 합니다.  

```js
PollOneResponse() -> static void

// example
// 유니티 객체의 Update 함수나 혹은 정기적(1초 이내)로 호출되는 코루틴 등에서 
// PollOneResponse 함수를 호출해 주세요.  
void Update() {
    SendQueue.PollOneResponse();
}
```

## SendQueue에 함수 삽입

SendQueue에 호출할 뒤끝 함수를 삽입합니다.  
SendQueue가 실행상태이면, 삽입된 함수들은 삽입된 순서대로 스레드에서 반환되어 실행됩니다.  
이때 뒤끝 함수 실행의 결과로 실행될 콜백 함수(인자 값으로 주어진 람다 함수)는 **Poll 함수**를 통해 메인 스레드에서 실행되게 됩니다.  

```js
Enqueue(Func<BackendReturnObject> BackendFunc, Action<BackendReturnObject> callback) -> static void
Enqueue<T1>(Func<T1, BackendReturnObject> BackendFunc, T1 GT, Action<BackendReturnObject> callback) -> static void
Enqueue<T1, T2>(Func<T1, BackendReturnObject> BackendFunc, T1 GT1, T2 GT2, Action<BackendReturnObject> callback) -> static void
Enqueue<T1, T2, T3>(Func<T1, BackendReturnObject> BackendFunc, , T1 GT1, T2 GT2, T3 GT3, Action<BackendReturnObject> callback) -> static void
Enqueue<T1, T2, T3, T4>(Func<T1, BackendReturnObject> BackendFunc, , T1 GT1, T2 GT2, T3 GT3, T4 GT4 Action<BackendReturnObject> callback) -> static void
Enqueue<T1, T2, T3, T4, T5>(Func<T1, BackendReturnObject> BackendFunc, , T1 GT1, T2 GT2, T3 GT3, T4 GT4, T5 GT5 Action<BackendReturnObject> callback) -> static void

// example
SendQueue.Enqueue(Backend.GameInfo.GetPublicContents, "publicTable", callback => {
    // 이후 처리
});
```

## 현재 SendQueue의 크기

SendQueue에 처리되지 않고 남아있는 함수의 개수를 반환합니다.  

```js
UnprocessedFuncCount -> static int

// example
int count = SendQueue.UnprocessedFuncCount;
```
