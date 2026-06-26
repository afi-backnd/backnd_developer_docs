---
sidebar_label: Server
---

# [Server]

메서드를 서버에서만 실행하도록 지정합니다. 핵심 게임 로직을 서버에서 안전하게 실행하여 치팅을 방지할 수 있습니다.

## 주요 특징
- 서버에서만 실행됩니다
- 클라이언트에서 호출하면 경고가 발생합니다
- Unity의 게임 루프 메서드(Start, Update 등)와 일반 메서드에서 모두 사용할 수 있습니다
- 상속받은 메서드에서 사용할 경우 override 메서드에도 속성을 명시해야 합니다

## 사용 예시

### 기본 사용법
```csharp
public class GameManager : NetworkBehaviour
{
    [Server]
    void Update()
    {
        // 서버에서만 실행되는 로직
        // 예: 게임 상태 업데이트
        UpdateGameState();
    }

    [Server]
    private void UpdateGameState()
    {
        // 게임 상태 업데이트 로직
        // 필요한 경우 ClientRpc를 통해 클라이언트에 알림
    }
}
```

### 상속된 메서드에서 사용
```csharp
public abstract class BaseEnemy : NetworkBehaviour
{
    [Server]
    protected virtual void HandleAI() 
    {
        // 기본 AI 로직
    }
}

public class SpecificEnemy : BaseEnemy
{
    [Server]
    protected override void HandleAI()
    {
        base.HandleAI();
        // 추가 AI 로직
    }
}
```

### 네트워크 스폰
```csharp
public class SpawnManager : NetworkBehaviour
{
    public GameObject enemyPrefab;

    [Server]
    private void SpawnEnemy()
    {
        // [Server] 어트리뷰트로 서버에서만 실행됨
        GameObject enemy = Instantiate(enemyPrefab);
        NetworkServer.Spawn(enemy);
    }
}
```

:::note
1. `[Server]` 속성이 있는 메서드는 서버에서만 실행되어야 하는 로직을 포함해야 합니다.
2. 클라이언트에게 정보를 전달해야 할 경우 `[ClientRpc]`를 사용합니다.
3. AI 처리, 게임 로직 처리, 오브젝트 스폰 등이 대표적인 사용 예시입니다.
:::
