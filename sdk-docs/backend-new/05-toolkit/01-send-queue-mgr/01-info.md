---
sidebar_label: "[Deprecated] SendQueueMgr"
---

# [Deprecated] SendQueueMgr

:::warning 신규 버전(6.0.1이상) SDK에서는 SendQueue를 지원하지 않습니다.
신규 버전(6.0.1이상) SDK에서는 더 이상 콜백을 위한 SendQueue를 지원하지 않습니다.  
async/await 로 동작하는 비동기 메서드가 추가되어 더 쉽게 SendQueue와 동일한 동작 구현이 가능합니다.  
async/await 비동기 메서드를 사용해 주시기 바랍니다.
:::

SendQueueMgr는 SendQueue를 손쉽게 사용하기 위해 제공되는 스크립트입니다.  
SendQueueMgr.cs를 유니티 객체에 적용하는 것만으로 `SendQueue`를 사용하기 위한 모든 준비가 완료됩니다.  

## SendQueueMgr 소개

- 스크립트를 객체에 적용하는 것만으로 게임의 상태에 따라 `SendQueue` 초기화 및 시작, 정지, 일시 정지, 재시작을 수행할 수 있습니다.  
- 유니티의 [DontDestroyOnLoad](https://docs.unity3d.com/kr/current/ScriptReference/Object.DontDestroyOnLoad.html)가 적용되어 씬을 새로 로드할 때에도 해당 객체가 파괴되지 않습니다.  
- 간단한 스크립트 형태로 제공되므로 개발사에서 원하는 방법으로 손쉽게 커스텀이 가능합니다.  

## SendQueueMgr 적용

### 1. `유니티 프로젝트 내 뒤끝 SDK 설치폴더(기본 경로 Assets>TheBackend) > ToolKit 폴더`를 확인해보면 **SendQueueMgr.cs**가 존재합니다.  

> ![image](https://developer.thebackend.io/static/img/ToolKit/sendQueue/1.png)

> ![image](https://developer.thebackend.io/static/img/ToolKit/sendQueue/2.png)
>   
  


### 2. 해당 스크립트를 유니티 씬 내 존재하는 객체에 적용하는 것으로 SendQueueMgr이 적용됩니다.  

> ![image](https://developer.thebackend.io/static/img/ToolKit/sendQueue/3.png)

## Exception 이벤트 처리

Exception이 발생 시 SendQueueMgr.cs 내 **ExcetionEvent(Exception e)** 함수가 호출됩니다.  
기본적으로는 발생한 Exception을 Debug.Log 함수를 이용하여 표시하게 되어있습니다.  
개발사에서 원하는 Exception 처리 로직을 추가해보세요.  

## SendQueue에 함수 삽입

기존 SendQueue 이용방법과 동일한 방법으로 SendQueue에 함수 삽입이 가능합니다.  
뒤끝 기능 호출을 원하는 스크립트에서 아래 함수를 호출하면 SendQueue에 함수를 삽입할 수 있습니다.  

```js
Enqueue(Func<BackendReturnObject> BackendFunc, Action<BackendReturnObject> callback) -> static void
Enqueue<T1>(Func<T1, BackendReturnObject> BackendFunc, T1 GT, Action<BackendReturnObject> callback) -> static void
Enqueue<T1, T2>(Func<T1, BackendReturnObject> BackendFunc, T1 GT1, T2 GT2, Action<BackendReturnObject> callback) -> static void
Enqueue<T1, T2, T3>(Func<T1, BackendReturnObject> BackendFunc, , T1 GT1, T2 GT2, T3 GT3, Action<BackendReturnObject> callback) -> static void
Enqueue<T1, T2, T3, T4>(Func<T1, BackendReturnObject> BackendFunc, , T1 GT1, T2 GT2, T3 GT3, T4 GT4 Action<BackendReturnObject> callback) -> static void
Enqueue<T1, T2, T3, T4, T5>(Func<T1, BackendReturnObject> BackendFunc, , T1 GT1, T2 GT2, T3 GT3, T4 GT4, T5 GT5 Action<BackendReturnObject> callback) -> static void

// example
SendQueue.Enqueue(Backend.GameInfo.GetPublicContents, "publicTable", callback =>
{
    // 이후 처리
});
```
