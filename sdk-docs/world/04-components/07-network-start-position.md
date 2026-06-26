---
sidebar_label: NetworkStartPosition
---

# NetworkStartPosition

플레이어 스폰 위치를 지정하는 컴포넌트입니다. 씬에 배치하면 플레이어 접속 시 해당 위치에 스폰됩니다.

## 기본 사용법

1. 빈 GameObject 생성
2. NetworkStartPosition 컴포넌트 추가
3. 원하는 스폰 위치로 Transform 설정

## 스폰 방식 설정

NetworkManager에서 플레이어 스폰 방식을 설정할 수 있습니다.

### PlayerSpawnMethod

```csharp
public enum PlayerSpawnMethod
{
    Random,      // 등록된 위치 중 무작위 선택
    RoundRobin   // 등록된 위치를 순서대로 선택
}
```

### Inspector 설정

NetworkManager의 **Player Spawn Method** 옵션:
- **Random**: 매번 무작위 스폰 위치 선택
- **Round Robin**: 순차적으로 스폰 위치 선택 (균등 분배)

## 다중 스폰 포인트 설정

여러 개의 NetworkStartPosition을 배치하여 다양한 스폰 위치를 제공할 수 있습니다.

```
씬 구조 예시:
├── SpawnPoints
│   ├── SpawnPoint_1 (NetworkStartPosition)
│   ├── SpawnPoint_2 (NetworkStartPosition)
│   ├── SpawnPoint_3 (NetworkStartPosition)
│   └── SpawnPoint_4 (NetworkStartPosition)
```

## NetworkScene에서의 사용

NetworkScene(던전)에도 NetworkStartPosition을 배치하면 해당 씬으로 이동한 플레이어가 자동으로 해당 위치에 스폰됩니다.

```
던전 씬 구조:
├── DungeonEnvironment
├── DungeonSpawnPoint (NetworkStartPosition)  ← 던전 입구
└── Monsters
```

:::note
1. NetworkStartPosition이 없으면 플레이어는 프리팹의 기본 Transform 값으로 스폰됩니다
2. 스폰 위치의 회전값도 플레이어에게 적용됩니다
3. 스폰 위치는 다른 오브젝트와 겹치지 않도록 충분한 간격을 두세요
:::
