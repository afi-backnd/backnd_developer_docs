---
sidebar_label: Client
description: "Client"
---

# [Client]

메서드를 클라이언트에서만 실행하도록 지정합니다. 속성 하나로 실행 환경을 제어할 수 있어 조건 분기 코드가 필요 없습니다.

## 주요 특징
- 클라이언트에서만 실행됩니다
- 서버에서 호출하면 경고가 발생합니다
- Unity의 게임 루프 메서드(Start, Update 등)와 일반 메서드에서 모두 사용할 수 있습니다
- 상속받은 메서드에서 사용할 경우 override 메서드에도 속성을 명시해야 합니다

## 사용 예시

### 기본 사용법
```csharp
public class PlayerController : NetworkBehaviour
{
    [Client]
    void Update()
    {
        // 클라이언트에서만 실행되는 로직
        // 예: 플레이어 입력 처리
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Jump();
        }
    }

    [Client]
    private void Jump()
    {
        // 점프 관련 로직
        // 실제 이동은 Command를 통해 서버에 요청
    }
}
```

### 상속된 메서드에서 사용
```csharp
public abstract class BasePlayer : NetworkBehaviour
{
    [Client]
    protected virtual void HandleInput() 
    {
        // 기본 입력 처리 로직
    }
}

public class SpecificPlayer : BasePlayer
{
    [Client]
    protected override void HandleInput()
    {
        base.HandleInput();
        // 추가 입력 처리 로직
    }
}
```

### 여러 클라이언트 전용 메서드 사용
```csharp
public class PlayerUI : NetworkBehaviour
{
    [Client]
    private void UpdateUI()
    {
        // [Client] 어트리뷰트로 클라이언트에서만 실행됨
        UpdateHealthBar();
        UpdateAmmoDisplay();
    }

    [Client]
    private void UpdateHealthBar()
    {
        // 체력바 업데이트 로직
    }

    [Client]
    private void UpdateAmmoDisplay()
    {
        // 탄약 표시 업데이트 로직
    }
}
```

:::note
1. `[Client]` 속성이 있는 메서드는 클라이언트에서만 실행되어야 하는 로직을 포함해야 합니다.
2. 실제 게임 상태 변경은 `[Command]`를 통해 서버에 요청해야 합니다.
3. UI 업데이트, 입력 처리, 시각/음향 효과 등이 대표적인 사용 예시입니다.
:::
