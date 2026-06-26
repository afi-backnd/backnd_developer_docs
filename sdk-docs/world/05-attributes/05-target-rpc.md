---
sidebar_label: TargetRpc
description: "TargetRpc"
---

# [TargetRpc]

서버에서 특정 클라이언트에게만 함수를 호출합니다. 개인 알림, 비공개 정보 전달 등 특정 플레이어에게만 메시지를 보낼 때 사용합니다.

## 주요 특징
- 서버에서 특정 클라이언트로의 호출만 가능합니다
- 지정된 클라이언트에서만 실행됩니다
- 함수 이름은 반드시 `Target` 접두사로 시작해야 합니다
- 첫 번째 매개변수는 반드시 NetworkConnection 타입이어야 합니다

## 사용 예시

### 기본 사용법
```csharp
public class PlayerNotification : NetworkBehaviour
{
    [Server]
    private void NotifyPlayer(NetworkConnection target)
    {
        TargetShowMessage(target, "접속을 환영합니다!");
    }

    [TargetRpc]
    private void TargetShowMessage(NetworkConnection target, string message)
    {
        // 특정 클라이언트에서만 실행되는 로직
        // UI 메시지 표시, 사운드 재생 등
    }
}
```

### 개인화된 정보 전달
```csharp
public class InventorySystem : NetworkBehaviour
{
    [Server]
    private void SendInventoryUpdate(NetworkConnection player, List<Item> items)
    {
        TargetUpdateInventory(player, items);
    }

    [TargetRpc]
    private void TargetUpdateInventory(NetworkConnection target, List<Item> items)
    {
        // 특정 플레이어의 인벤토리 UI 업데이트
    }
}
```

### 개별 알림 전송
```csharp
public class QuestManager : NetworkBehaviour
{
    [Server]
    private void QuestCompleted(NetworkConnection player, int questId)
    {
        TargetShowQuestComplete(player, questId);
    }

    [TargetRpc]
    private void TargetShowQuestComplete(NetworkConnection target, int questId)
    {
        // 퀘스트 완료 UI 표시
        // 보상 획득 애니메이션 재생
        // 효과음 재생 등
    }
}
```

:::note
1. TargetRpc는 특정 클라이언트에게만 정보를 전달할 때 사용됩니다
2. 함수 이름은 반드시 `Target` 접두사로 시작해야 합니다
3. 첫 번째 매개변수로 NetworkConnection을 받아야 합니다
4. 개인화된 메시지, 특정 플레이어 상태 업데이트 등에 주로 사용됩니다
:::
