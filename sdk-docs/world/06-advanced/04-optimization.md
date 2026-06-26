---
sidebar_label: 네트워크 최적화
description: "네트워크 최적화"
---

# 네트워크 최적화

자동 배칭, 델타 동기화 등 기본 최적화 기능이 적용 되어 있습니다. 대부분의 게임에서는 별도 설정 없이도 충분한 성능을 얻을 수 있습니다. 그러나 대규모 멀티플레이, 빈번한 오브젝트 생성/삭제, 실시간 액션 등에서는 추가적인 최적화가 필요할 수 있습니다.  

아래 항목을 참고해 네트워크 트래픽과 서버 부하를 효과적으로 관리하세요.

- **SyncVar/Command/RPC 최소화:** 꼭 필요한 데이터만 동기화하고, 불필요한 네트워크 호출을 줄이세요.
- **오브젝트 풀링:** 빈번히 생성/삭제되는 오브젝트는 풀링으로 관리해 네트워크와 서버 부하를 완화하세요.
- **AI/물리 연산 최적화:** 서버에서만 처리해야 하는 연산의 빈도를 제한하세요.

실제 적용 예시는 아래 각 항목에서 확인할 수 있습니다.

## 대역폭 최적화

### 1. SyncVar 최적화

#### 필요한 것만 동기화

```csharp
public class Player : NetworkBehaviour
{
    // ❌ 잘못된 예: 불필요하게 동기화
    [SyncVar]
    public string debugInfo;  // 디버그용 정보는 동기화 불필요

    // ✅ 올바른 예: 게임플레이에 필요한 것만 동기화
    [SyncVar]
    public int health;

    [SyncVar]
    public int score;
}
```

### 2. RPC 최적화

#### 배칭(Batching) 활용

```csharp
// ❌ 잘못된 예: 여러 번의 RPC 호출
[Server]
void BadApplyDamage(List<int> targetIds, int damage)
{
    foreach (int id in targetIds)
    {
        RpcShowDamage(id, damage);  // 반복 호출
    }
}

// ✅ 올바른 예: 한 번의 RPC로 배칭
[Server]
void GoodApplyDamage(List<int> targetIds, int damage)
{
    RpcShowDamageMultiple(targetIds.ToArray(), damage);
}

[ClientRpc]
void RpcShowDamageMultiple(int[] targetIds, int damage)
{
    foreach (int id in targetIds)
    {
        ShowDamageEffect(id, damage);
    }
}
```

#### TargetRpc로 대상 제한

```csharp
// ❌ 잘못된 예: 모든 클라이언트에게 전송
[ClientRpc]
void RpcShowPrivateMessage(string message, uint targetPlayerId)
{
    if (netIdentity.netId == targetPlayerId)
    {
        ShowMessage(message);
    }
}

// ✅ 올바른 예: 특정 클라이언트에게만 전송
[TargetRpc]
void TargetShowPrivateMessage(NetworkConnection target, string message)
{
    ShowMessage(message);
}
```

## 서버 부하 최적화

### 1. 물리 최적화

```csharp
public class ServerPhysics : NetworkBehaviour
{
    [Server]
    void FixedUpdate()
    {
        // [Server] 어트리뷰트로 서버에서만 실행됨
        ProcessPhysics();
    }

    [Client]
    void Update()
    {
        // [Client] 어트리뷰트로 클라이언트에서만 실행됨
        UpdateVisuals();
    }
}
```

### 2. AI 최적화

```csharp
public class OptimizedAI : NetworkBehaviour
{
    private float aiUpdateInterval = 0.2f;  // AI 업데이트 간격
    private float lastAIUpdate;

    [Server]
    void Update()
    {
        // AI 업데이트 빈도 제한
        if (Time.time - lastAIUpdate < aiUpdateInterval) return;
        lastAIUpdate = Time.time;

        // 가시 범위 내 관찰자가 없으면 AI 비활성화
        if (netIdentity.observers.Count == 0) return;

        UpdateAI();
    }
}
```

## 오브젝트 풀링

### NetworkServer.Spawn 최적화

```csharp
public class ProjectilePool : NetworkBehaviour
{
    public GameObject projectilePrefab;
    private Queue<GameObject> pool = new Queue<GameObject>();

    [Server]
    public GameObject GetProjectile()
    {
        GameObject projectile;

        if (pool.Count > 0)
        {
            projectile = pool.Dequeue();
            projectile.SetActive(true);
            NetworkServer.Spawn(projectile);  // 재사용 시에도 Spawn 필요
        }
        else
        {
            projectile = Instantiate(projectilePrefab);
            NetworkServer.Spawn(projectile);
        }

        return projectile;
    }

    [Server]
    public void ReturnProjectile(GameObject projectile)
    {
        NetworkServer.UnSpawn(projectile);  // 클라이언트에서 제거
        projectile.SetActive(false);
        projectile.transform.position = Vector3.zero;
        pool.Enqueue(projectile);
    }
}
```

## 체크리스트

### 출시 전 최적화 체크리스트

- [ ] 불필요한 SyncVar 제거
- [ ] RPC 호출 빈도 확인
- [ ] 오브젝트 풀링 적용
- [ ] AI 업데이트 빈도 제한

:::warning 주의사항
과도한 최적화로 게임플레이 품질을 저하시키지 마세요.
:::
