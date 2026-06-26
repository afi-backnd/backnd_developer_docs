---
sidebar_label: ClientRpc
description: "ClientRpc"
---

# [ClientRpc]

서버에서 모든 클라이언트로 함수를 호출합니다. 게임 이벤트나 상태 변경을 모든 플레이어에게 전파할 때 사용합니다.

## 주요 특징
- 서버에서 모든 클라이언트로의 호출만 가능합니다
- 클라이언트에서 실행됩니다
- 함수 이름은 반드시 `Rpc` 접두사로 시작해야 합니다
- 연결된 모든 클라이언트에서 실행됩니다

## 사용 예시

### 기본 사용법
```csharp
public class GameManager : NetworkBehaviour
{
    [Server]
    private void StartGame()
    {
        // 게임 시작 로직
        RpcNotifyGameStart();
    }

    [ClientRpc]
    private void RpcNotifyGameStart()
    {
        // 클라이언트에서 실행되는 로직
        // UI 업데이트, 사운드 재생 등
    }
}
```

### 매개변수 전달
```csharp
public class PlayerStatus : NetworkBehaviour
{
    [Server]
    public void TakeDamage(int damage)
    {
        // 데미지 처리 로직
        RpcUpdateHealth(currentHealth);
    }

    [ClientRpc]
    private void RpcUpdateHealth(int newHealth)
    {
        // 클라이언트에서 체력 상태 갱신
        // UI 업데이트, 이펙트 재생 등
    }
}
```

### 여러 정보 동기화
```csharp
public class WorldState : NetworkBehaviour
{
    [Server]
    private void UpdateWeather(WeatherType weather, float intensity)
    {
        // 날씨 상태 변경 로직
        RpcSyncWeather(weather, intensity);
    }

    [ClientRpc]
    private void RpcSyncWeather(WeatherType weather, float intensity)
    {
        // 클라이언트의 날씨 상태 갱신
        // 파티클 효과, 사운드 등 업데이트
    }
}
```

:::note
1. ClientRpc는 서버의 상태 변경을 모든 클라이언트에게 알리기 위해 사용됩니다
2. 함수 이름은 반드시 `Rpc` 접두사로 시작해야 합니다
3. 게임 상태 변경, 이벤트 알림, 환경 변화 등을 클라이언트에게 전달할 때 주로 사용됩니다
:::
