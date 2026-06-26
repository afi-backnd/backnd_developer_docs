---
sidebar_label: SyncVar
description: "SyncVar"
---

# [SyncVar]

서버에서 클라이언트로 변수를 동기화합니다. 속성만 추가하면 서버에서 변경된 값이 모든 클라이언트에 전파됩니다.

동기화 코드 없이 변수 선언만으로 멀티플레이어 상태 관리가 가능합니다.

## 주요 특징
- 서버에서 클라이언트로의 단방향 동기화입니다
- 클라이언트에서의 변경은 의미가 없습니다
- null 값은 사용할 수 없습니다
- 기본 타입(int, long, float, string, Vector3 등)과 NetworkIdentity를 포함한 GameObject를 지원합니다

## 기본 사용법

### 일반 변수 동기화
```csharp
public class Player : NetworkBehaviour
{
    [SyncVar]
    public int health = 100;

    [SyncVar]
    public string playerName = "Player";

    [SyncVar]
    public Vector3 spawnPoint;
}
```

### Hook 함수 사용
```csharp
public class Player : NetworkBehaviour
{
    [SyncVar(hook = nameof(OnHealthChanged))]
    private int health = 100;

    // hook 함수는 이전 값과 새로운 값을 매개변수로 받습니다
    private void OnHealthChanged(int oldValue, int newValue)
    {
        // 체력 변경 시 실행할 로직
        UpdateHealthUI(newValue);
        PlayDamageEffect(oldValue - newValue);
    }
}
```

### NetworkIdentity가 있는 GameObject 동기화
```csharp
public class PlayerEquipment : NetworkBehaviour
{
    [SyncVar]
    public GameObject currentWeapon;  // NetworkIdentity가 있는 GameObject만 가능

    [Server]
    public void EquipWeapon(GameObject weapon)
    {
        if (!weapon.GetComponent<NetworkIdentity>())
            return;
            
        currentWeapon = weapon;
    }
}
```

### Hook과 NetworkIdentity 함께 사용
```csharp
public class PlayerParty : NetworkBehaviour
{
    [SyncVar(hook = nameof(OnPartyChanged))]
    public GameObject currentParty;  // NetworkIdentity가 있는 파티 오브젝트

    private void OnPartyChanged(GameObject oldParty, GameObject newParty)
    {
        // 파티 변경 시 실행할 로직
        UpdatePartyUI();
        UpdatePartyMarkers();
    }
}
```

:::note
1. SyncVar는 서버에서만 값을 변경해야 합니다
2. 클라이언트에서 SyncVar 값을 변경해도 동기화되지 않습니다
3. Hook 함수는 값이 변경될 때마다 자동으로 호출됩니다
4. GameObject를 SyncVar로 사용할 때는 반드시 NetworkIdentity 컴포넌트가 필요합니다
:::
