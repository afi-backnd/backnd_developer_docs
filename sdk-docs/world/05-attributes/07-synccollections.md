---
sidebar_label: SyncVar Collections
---

# SyncVar Collections

네트워크에서 컬렉션을 동기화하는 특수 타입입니다. SyncList, SyncDictionary 등을 사용하면 서버에서 변경된 컬렉션이 모든 클라이언트에 전파됩니다.

인벤토리, 퀘스트 목록, 장비 슬롯 등 복잡한 데이터 구조를 쉽게 동기화할 수 있습니다.

## SyncList
순서가 있는 데이터 목록을 동기화할 때 사용합니다. 퀘스트 목록, 인벤토리 등을 구현할 때 유용합니다.

```csharp
public class Inventory : NetworkBehaviour
{
    public readonly SyncList<int> items = new SyncList<int>();

    public override void OnStartClient()
    {
        items.OnAdd += OnItemAdded;
        items.OnRemove += OnItemRemoved;

        // 초기 인벤토리 로드
        for (int i = 0; i < items.Count; i++)
            OnItemAdded(i);
    }

    public override void OnStopClient()
    {
        items.OnAdd -= OnItemAdded;
        items.OnRemove -= OnItemRemoved;
    }

    [Server]
    public void AddItem(int itemId)
    {
        items.Add(itemId);
    }

    void OnItemAdded(int index)
    {
        // 새 아이템 획득 UI 표시
        UIManager.Instance.ShowNewItemNotification(items[index]);
    }

    void OnItemRemoved(int index, int oldValue)
    {
        // 인벤토리 UI 업데이트
        UIManager.Instance.UpdateInventoryUI();
    }
}
```

## SyncDictionary
키-값 쌍의 데이터를 동기화할 때 사용합니다. 장비 시스템, 스킬 시스템 등을 구현할 때 유용합니다.

```csharp
public enum EquipSlot { Weapon, Armor, Helmet, Boots }

public struct EquipItem
{
    public int itemId;
    public int enhanceLevel;
    public bool isLocked;
}

public class EquipmentSystem : NetworkBehaviour
{
    public readonly SyncDictionary<EquipSlot, EquipItem> equipment = new SyncDictionary<EquipSlot, EquipItem>();

    public override void OnStartClient()
    {
        equipment.OnSet += OnEquipmentChanged;
        equipment.OnRemove += OnEquipmentRemoved;

        // 초기 장비 로드
        foreach (var slot in equipment.Keys)
            OnEquipmentChanged(slot, default);
    }

    public override void OnStopClient()
    {
        equipment.OnSet -= OnEquipmentChanged;
        equipment.OnRemove -= OnEquipmentRemoved;
    }

    [Server]
    public void EquipItem(EquipSlot slot, EquipItem item)
    {
        equipment[slot] = item;
    }

    [Server]
    public void UnequipItem(EquipSlot slot)
    {
        equipment.Remove(slot);
    }

    void OnEquipmentChanged(EquipSlot slot, EquipItem oldItem)
    {
        // 장비 변경 효과 표시
        VFXManager.Instance.PlayEquipEffect(slot);
        // 캐릭터 외형 업데이트
        CharacterCustomization.Instance.UpdateEquipment(slot, equipment[slot]);
    }

    void OnEquipmentRemoved(EquipSlot slot, EquipItem oldItem)
    {
        // 장비 해제 효과 표시
        VFXManager.Instance.PlayUnequipEffect(slot);
        // 캐릭터 외형 초기화
        CharacterCustomization.Instance.ClearEquipment(slot);
    }
}
```

## SyncHashSet
중복되지 않는 데이터 집합을 동기화할 때 사용합니다. 획득한 업적, 해금된 콘텐츠 등을 구현할 때 유용합니다.

```csharp
public class AchievementSystem : NetworkBehaviour
{
    public readonly SyncHashSet<string> unlockedAchievements = new SyncHashSet<string>();

    public override void OnStartClient()
    {
        unlockedAchievements.OnAdd += OnAchievementUnlocked;

        // 초기 업적 로드
        foreach (string achievementId in unlockedAchievements)
            OnAchievementUnlocked(achievementId);
    }

    public override void OnStopClient()
    {
        unlockedAchievements.OnAdd -= OnAchievementUnlocked;
    }

    [Server]
    public void UnlockAchievement(string achievementId)
    {
        if (unlockedAchievements.Add(achievementId))
        {
            // 업적 달성 보상 지급 등의 서버 로직
        }
    }

    void OnAchievementUnlocked(string achievementId)
    {
        // 업적 달성 UI 표시
        UIManager.Instance.ShowAchievementPopup(achievementId);
        // 업적 효과음 재생
        AudioManager.Instance.PlayAchievementSound();
    }
}
```

## SyncSortedSet
자동으로 정렬되는 중복되지 않는 데이터 집합을 동기화할 때 사용합니다. 랭킹 시스템, 점수판 등을 구현할 때 유용합니다.

```csharp
public struct ScoreData : IComparable<ScoreData>
{
    public string playerName;
    public int score;

    public int CompareTo(ScoreData other) => other.score.CompareTo(score); // 높은 점수순
}

public class LeaderboardSystem : NetworkBehaviour
{
    public readonly SyncSortedSet<ScoreData> leaderboard = new SyncSortedSet<ScoreData>();

    public override void OnStartClient()
    {
        leaderboard.OnAdd += OnScoreAdded;
        leaderboard.OnRemove += OnScoreRemoved;

        // 초기 리더보드 로드
        UpdateLeaderboardUI();
    }

    public override void OnStopClient()
    {
        leaderboard.OnAdd -= OnScoreAdded;
        leaderboard.OnRemove -= OnScoreRemoved;
    }

    [Server]
    public void AddScore(string playerName, int score)
    {
        leaderboard.Add(new ScoreData { playerName = playerName, score = score });
    }

    [Server]
    public void RemoveScore(ScoreData score)
    {
        leaderboard.Remove(score);
    }

    void OnScoreAdded(ScoreData score)
    {
        // 리더보드 UI 업데이트
        UpdateLeaderboardUI();
    }

    void OnScoreRemoved(ScoreData score)
    {
        // 리더보드 UI 업데이트
        UpdateLeaderboardUI();
    }

    void UpdateLeaderboardUI()
    {
        UIManager.Instance.UpdateLeaderboard(leaderboard);
    }
}
```

## 다형성 지원 (상속 구조 사용)

SyncVar 컬렉션은 제네릭 타입 기준으로 직렬화되므로, 상속받은 클래스를 그대로 사용하면 부모 클래스로만 역직렬화됩니다.

```csharp
// ❌ 문제 상황
public class BaseClass { public int id; }
public class ChildClass : BaseClass { public string name; }

public readonly SyncDictionary<string, BaseClass> data = new();
data["key"] = new ChildClass { id = 1, name = "test" };

// 클라이언트에서 수신 후
var obj = data["key"];
var child = obj as ChildClass;  // null! - BaseClass로만 역직렬화됨
```

### 해결 방법: 래퍼 구조체 사용

타입 정보와 JSON 데이터를 함께 저장하는 래퍼를 사용합니다.

```csharp
using System;
using UnityEngine;

[Serializable]
public struct PolyData
{
    public string typeName;
    public string jsonData;

    // 저장: BaseClass 또는 자식 클래스 → PolyData 자동 변환
    public static implicit operator PolyData(BaseClass obj)
    {
        if (obj == null) return default;
        return new PolyData
        {
            typeName = obj.GetType().AssemblyQualifiedName,
            jsonData = JsonUtility.ToJson(obj)
        };
    }

    // 꺼내기: 원래 타입으로 복원
    public T Get<T>() where T : BaseClass
    {
        if (string.IsNullOrEmpty(typeName)) return null;
        return JsonUtility.FromJson(jsonData, Type.GetType(typeName)) as T;
    }

    public BaseClass Get() => Get<BaseClass>();
}
```

### 사용 예제

```csharp
public class DataManager : NetworkBehaviour
{
    public readonly SyncDictionary<string, PolyData> data = new SyncDictionary<string, PolyData>();

    public override void OnStartClient()
    {
        data.OnSet += OnDataChanged;

        foreach (var key in data.Keys)
            OnDataChanged(key, default);
    }

    public override void OnStopClient()
    {
        data.OnSet -= OnDataChanged;
    }

    // 1. 클라이언트에서 Command 호출
    [Command]
    public void CmdSetPlayerData(string key, PolyData value)
    {
        SetPlayerData(key, value);
    }

    // 2. 서버에서 SyncDictionary 변경
    [Server]
    public void SetPlayerData(string key, PolyData value)
    {
        data[key] = value;
    }

    // 3. 클라이언트에서 Hook 실행
    void OnDataChanged(string key, PolyData oldValue)
    {
        BaseClass obj = data[key].Get();

        if (obj is ChildClass child)
        {
            Debug.Log($"ChildClass: {child.name}");
        }
        else if (obj is OtherChild other)
        {
            Debug.Log($"OtherChild: {other.value}");
        }
    }
}
```

```csharp
// 클라이언트에서 호출
var child = new ChildClass { id = 1, name = "test" };
dataManager.CmdSetPlayerData("player1", child);  // 암시적 변환으로 PolyData 전달
```

:::caution
PolyData는 JsonUtility를 사용하므로 다음 제한사항이 있습니다:
- Dictionary, 다차원 배열 등 일부 타입은 직렬화되지 않습니다
- private 필드는 `[SerializeField]` 속성이 필요합니다
- 프로퍼티는 직렬화되지 않습니다
:::

:::note
1. 모든 SyncVar 컬렉션은 서버에서만 수정해야 합니다
2. 클라이언트는 OnStartClient에서 이벤트 핸들러를 등록하고, OnStopClient에서 제거해야 합니다
3. 초기 데이터는 이벤트 핸들러 등록 전에 이미 동기화되어 있으므로, 필요한 경우 수동으로 처리해야 합니다
4. OnChange는 범용 이벤트이므로 가능한 구체적인 이벤트(OnAdd, OnRemove 등)를 사용하는 것이 좋습니다
5. 컬렉션은 readonly로 선언하여 인스턴스가 변경되는 것을 방지합니다
:::
