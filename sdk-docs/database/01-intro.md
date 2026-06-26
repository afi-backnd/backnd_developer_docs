---
sidebar_label: 시작하기
description: "시작하기"
---

# 시작하기

데이터베이스는 유니티 개발자에게 친숙한 **LINQ(Language Integrated Query)** 문법을 사용하여 서버의 데이터를 손쉽게 관리할 수 있는 기능을 제공합니다.

복잡한 쿼리 언어를 배우지 않고도 C# 코드로 강력한 데이터베이스 기능을 활용할 수 있습니다.

## 주요 특징

*   **Type-Safe**: 컴파일 시점에 타입 체크를 수행하여 런타임 오류를 방지합니다.
*   **강력한 보안**: 테이블별 권한 설정(`ClientAccess`, `Read`, `Write`)을 통해 클라이언트 주도 개발의 보안 취약점을 해결했습니다.
*   **목적별 테이블 지원**: 유저 데이터를 위한 `UserTable`과 공용 데이터를 위한 `FlexibleTable`을 제공합니다.
*   **인덱스 지원**: 자주 조회되는 컬럼에 인덱스를 설정하여 쿼리 성능을 최적화할 수 있습니다. [인덱스 생성 가이드](/guide/console-guide/database/table-editor/#테이블-수정)를 참고하세요.
*   **운영 관리 콘솔**: [뒤끝 콘솔 데이터 에디터](/guide/console-guide/database/table-editor/)을 통해 데이터베이스 스키마와 데이터를 시각적으로 관리하고, [Query Editor](/guide/console-guide/database/query-editor/)로 **SQL**를 직접 작성하여 서비스 운영을 효율적으로 수행할 수 있습니다.

## SDK 설치

### 시스템 요구사항

:::note Unity 호환성
데이터베이스는 다음 Unity LTS(Long Term Support) 버전을 공식 지원합니다:

- Unity 6 LTS (최소 버전: 6000.0.23f1)
- Unity 2022 LTS (최소 버전: 2022.2.5f1)
- Unity 2021 LTS (최소 버전: 2021.3.18f1)

안정적인 사용을 위해 각 버전의 최신 LTS 패치 사용을 권장합니다.
:::

:::note 참고: 베이스 SDK와 함께 사용하는 경우
베이스 SDK **5.17.0 이상** 버전이 설치가 이루어져 있거나 설치를 하셔야 정상적으로 데이터베이스 SDK를 사용할 수 있습니다.
:::


### 설치 단계

#### 1. Registry 설정하기

1. Unity 편집기에서 **Edit → Project Settings**로 이동합니다.
   ![프로젝트 설정 메뉴](/img/docs/guide/world/sdk/install/01.png)

2. Package Manager에서 'Enable Preview Packages' 옵션을 활성화합니다.
   ![프리뷰 패키지 활성화](/img/docs/guide/world/sdk/install/02.png)

3. Registry 정보를 다음과 같이 입력합니다:

   **Name:**
   ```
   backnd
   ```
   ![Name 입력](/img/docs/guide/world/sdk/install/03.png)

   **URL:**
   ```
   https://registry.npmjs.org
   ```
   ![URL 입력](/img/docs/guide/world/sdk/install/04.png)

   **Scope(s):**
   ```
   com.backnd
   ```
   ![Scope 입력](/img/docs/guide/world/sdk/install/05.png)

4. Save 버튼을 클릭하여 설정을 저장합니다.
   ![설정 저장](/img/docs/guide/world/sdk/install/06.png)

5. 정상 입력 예시
   ![프로젝트 설정](/img/docs/guide/database/sdk/project-settings.png)

#### 2. 패키지 설치하기

1. **Window → Package Manager**를 선택합니다.
   ![패키지 매니저](/img/docs/guide/world/sdk/install/07.png)

2. 드롭다운 메뉴에서 **My Registries**를 선택합니다.
   ![My Registries 선택](/img/docs/guide/world/sdk/install/08.png)

   :::note
   My Registries가 보이지 않는다면 Registry 설정이 올바르게 되었는지 확인해주세요.
   :::

3. 목록에서 패키지를 찾아 Install 버튼을 클릭합니다.
   ![패키지 설치](/img/docs/guide/world/sdk/install/09.png)

## 네임스페이스

SDK를 사용하기 위해 다음 네임스페이스를 추가해야 합니다.

```csharp
using BACKND.Database; // 데이터베이스 SDK
using BackEnd;         // 뒤끝 베이스 SDK
```

## 초기화

데이터베이스 기능을 사용하기 위해서는 먼저 `Client` 객체를 생성하고 초기화해야 합니다.
이 작업은 뒤끝 로그인(`Backend.BMember.Login` 등)이 성공한 이후에 수행해야 합니다.

```csharp
public class DatabaseManager : MonoBehaviour
{
    // 데이터베이스 클라이언트 인스턴스
    public static Client DBClient;

    public void InitializeDatabase()
    {
        // 1. 뒤끝 초기화 및 로그인 (선행 필수)
        Backend.InitializeAsync(bro =>
        {
            if (bro.IsSuccess())
            {
                Backend.BMember.CustomLogin("user", "password", async loginBro =>
                {
                    if (loginBro.IsSuccess())
                    {
                        // 2. 데이터베이스 클라이언트 생성
                        // 뒤끝 콘솔 > 데이터베이스 관리 메뉴에서 발급받은 DB UUID를 입력하세요.
                        DBClient = new Client("YOUR_DB_UUID");

                        // 3. 데이터베이스 초기화
                        await DBClient.Initialize();
                        
                        Debug.Log("데이터베이스 초기화 완료");
                    }
                });
            }
        });
    }
}
```

:::note 참고: 데이터베이스 SDK만 단독으로 사용하실 수 있습니다.
데이터베이스 SDK에는 유저 인증 기능이 포함된 `Backend.dll`이 기본 내장되어 있습니다.  
따라서 베이스 SDK를 별도로 설치하지 않아도, 데이터베이스만 단독으로 사용할 수 있습니다.
:::

## 기본 사용법

초기화된 클라이언트(`DBClient`)를 사용하여 쿼리를 작성할 수 있습니다.

```csharp
// 예시: 레벨 10 이상인 유저 조회
var users = await DBClient.From<User>()
    .Where(u => u.Level >= 10)
    .OrderByDescending(u => u.Experience)
    .Take(10)
    .ToList();
```

## 테이블 & 인덱스 제한 사항

데이터베이스 테이블 & 인덱스 설계 시 반드시 알아야 할 주요 제한사항은 다음과 같습니다.

| 항목                | 제한 내용                                                      |
|---------------------|---------------------------------------------------------------|
| 테이블 개수         | 데이터베이스당 최대 100개                                      |
| 컬럼 개수           | 테이블당 최대 80개                                            |
| JSON/string 컬럼    | 테이블당 최대 20개                                            |
| 인덱스 개수         | 데이터베이스당 최대 10개 |
| 인덱스 컬럼         | 인덱스 1개당 최대 4개 컬럼까지 묶어서 생성 가능                |
| 테이블 생성         | 1개 이상의 컬럼과 primary 필수                                 |

※ 자세한 제약 및 예외 사항은 콘솔 가이드의 [테이블 에디터], [데이터베이스 설정] 문서를 참고하세요.

[테이블 에디터]: /guide/console-guide/database/table-editor/
[데이터베이스 설정]: /guide/console-guide/database/settings/
