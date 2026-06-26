---
sidebar_label: 데이터 테이블
description: "데이터 테이블"
---

# Data Table

게임 데이터를 중앙에서 관리하는 시스템입니다. 콘솔에서 스프레드시트 형식으로 편집하면 CDN을 통해 모든 클라이언트에 배포됩니다.

아이템, 몬스터, 스킬 등 게임 콘텐츠 데이터를 코드 수정 없이 관리할 수 있습니다.

## 주요 기능
- 게임 콘텐츠 데이터를 중앙에서 관리
- 스프레드시트 형식으로 데이터 편집
- CDN을 통한 자동 배포
- LINQ 스타일의 강력한 쿼리 기능 제공

## 컴포넌트 설정

### Inspector
![Data Table Inspector](/img/docs/guide/world/sdk/data-table.png)
- **Refresh Data Tables**: 최신 데이터로 업데이트

### 컴포넌트 참조
```csharp
public class GameDataManager : MonoBehaviour
{
    private BACKND.DataTable _dataTable;

    void Awake()
    {
        _dataTable = GetComponent<BACKND.DataTable>();
        if (_dataTable == null)
        {
            _dataTable = gameObject.AddComponent<BACKND.DataTable>();
        }
    }
}
```

## 데이터 정의 및 사용

### 아이템 데이터 예시
```csv
id,name,type,grade,price,attack,defense
1001,Ancient Sword,Weapon,Legendary,5000,150,0
1002,Steel Armor,Armor,Rare,2000,0,80
1003,Health Potion,Consumable,Common,100,0,0
```

### 클래스 정의
```csharp
public class ItemData
{
    public int id;
    public string name;
    public string type;
    public string grade;
    public int price;
    public int attack;
    public int defense;
}
```

### 데이터 조회
```csharp
public class ItemManager : MonoBehaviour
{
    private BACKND.DataTable _dataTable;

    void Awake()
    {
        _dataTable = GetComponent<BACKND.DataTable>();
    }

    // 단일 아이템 조회
    public ItemData GetItem(int itemId)
    {
        return _dataTable.From<ItemData>()
            .Where(x => x.id == itemId)
            .FirstOrDefault();
    }

    // 특정 등급의 아이템 목록
    public List<ItemData> GetItemsByGrade(string grade)
    {
        return _dataTable.From<ItemData>()
            .Where(x => x.grade == grade)
            .ToList();
    }
}
```

## 실전 예시: 몬스터 데이터

### 데이터 구조
```csv
id,name,level,health,attack,defense
1,Goblin,1,100,10,5
2,Wolf,3,150,15,8
3,Dragon,10,1000,100,50
```

### 구현 예시
```csharp
public class MonsterData
{
    public int id;
    public string name;
    public int level;
    public int health;
    public int attack;
    public int defense;
}

public class MonsterManager : MonoBehaviour
{
    private BACKND.DataTable _dataTable;

    void Awake()
    {
        _dataTable = GetComponent<BACKND.DataTable>();
    }

    // 레벨 범위로 몬스터 찾기
    public List<MonsterData> GetMonstersByLevel(int minLevel, int maxLevel)
    {
        return _dataTable.From<MonsterData>()
            .Where(x => x.level >= minLevel && x.level <= maxLevel)
            .OrderBy(x => x.level)
            .ToList();
    }

    // 가장 강력한 몬스터 찾기
    public MonsterData GetStrongestMonster()
    {
        return _dataTable.From<MonsterData>()
            .OrderByDescending(x => x.attack)
            .FirstOrDefault();
    }
}
```

:::tip 사용 팁
1. **컴포넌트 설정**
   - DataTable 컴포넌트는 반드시 GetComponent로 참조해야 합니다
   - 없는 경우 AddComponent로 추가할 수 있습니다

2. **데이터 관리**
   - 콘솔에서 데이터 수정 후 Refresh 버튼으로 최신화
   - 연관된 데이터는 하나의 테이블로 관리

3. **쿼리 활용**
   - Where로 필요한 데이터만 필터링
   - OrderBy로 데이터 정렬
   - FirstOrDefault로 단일 데이터 조회
:::

:::note
- 테이블의 컬럼명과 클래스의 속성명이 일치해야 합니다
- Refresh Data Tables 버튼으로 최신 데이터를 불러올 수 있습니다
- CDN을 통해 자동으로 배포되므로 별도의 배포 과정이 필요하지 않습니다
:::
