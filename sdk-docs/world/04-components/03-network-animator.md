---
sidebar_label: NetworkAnimator
---

# NetworkAnimator

Unity Animator를 그대로 사용하면서 네트워크 동기화가 가능합니다. 컴포넌트만 추가하면 애니메이션 상태와 파라미터가 모든 클라이언트에 반영됩니다.

## 기본 설정

NetworkAnimator 컴포넌트를 사용하려면 다음 컴포넌트들이 필요합니다:
- NetworkIdentity 컴포넌트
- Animator 컴포넌트

## Inspector 설정

Inspector에서 다음 설정들을 구성할 수 있습니다:

### Client Authority
클라이언트에서 애니메이션을 제어할지 여부를 설정합니다:
- true: 클라이언트에서 애니메이션 파라미터 변경이 서버로 전송됩니다
- false: 서버에서만 애니메이션을 제어할 수 있습니다

### Animator
동기화할 Animator 컴포넌트를 지정하는 필드입니다.

## 트리거 사용법

:::note
Animator의 트리거는 직접 동기화되지 않습니다. 대신 `NetworkAnimator.SetTrigger`를 사용해야 합니다. 
```csharp
// 트리거 실행 예시
networkAnimator.SetTrigger("TriggerName");
```
:::
