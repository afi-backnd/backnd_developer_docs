---
sidebar_label: Command
---

# [Command]

클라이언트에서 서버로 함수를 호출합니다. 일반 함수 호출처럼 사용하면 되며, 네트워크 메시지 정의나 직렬화 코드가 필요 없습니다.

## 주요 특징
- 클라이언트에서 서버로의 호출만 가능합니다
- 서버에서 실행됩니다
- 함수 이름은 반드시 `Cmd` 접두사로 시작해야 합니다
- 호출한 클라이언트의 연결 정보를 자동으로 포함합니다

## 사용 예시

### 기본 사용법
```csharp
public class PlayerController : NetworkBehaviour
{
    [Client]
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            CmdRequestJump();
        }
    }

    [Command]
    private void CmdRequestJump()
    {
        // 서버에서 실행되는 점프 로직
        // 물리 처리, 위치 변경 등
    }
}
```

### 매개변수 전달
```csharp
public class PlayerCombat : NetworkBehaviour
{
    [Client]
    private void OnFireButtonPressed(Vector3 targetPosition)
    {
        CmdFireWeapon(targetPosition);
    }

    [Command]
    private void CmdFireWeapon(Vector3 targetPosition)
    {
        // 서버에서 실행되는 발사 로직
        // 탄환 생성, 충돌 처리 등
    }
}
```

### 서버 검증
```csharp
public class PlayerInventory : NetworkBehaviour
{
    [Command]
    private void CmdUseItem(int itemId)
    {
        // 아이템 보유 여부 확인 (서버에서 검증)
        if (!HasItem(itemId))
            return;

        // 아이템 사용 로직
        UseItemOnServer(itemId);
    }
}
```

:::note
1. Command는 클라이언트의 요청을 서버에서 안전하게 처리하기 위해 사용됩니다
2. 함수 이름은 반드시 `Cmd` 접두사로 시작해야 합니다
3. 입력 처리, 아이템 사용, 공격 등 플레이어의 행동을 서버에 전달할 때 주로 사용됩니다
:::
