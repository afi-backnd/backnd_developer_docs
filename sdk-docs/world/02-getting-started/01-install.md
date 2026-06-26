---
sidebar_label: SDK 설치
---

# SDK 설치

## 시스템 요구사항

:::note Unity 호환성
월드는 다음 Unity LTS(Long Term Support) 버전을 공식 지원합니다:

- Unity 6 LTS (최소 버전: 6000.0.23f1)
- Unity 2022 LTS (최소 버전: 2022.2.5f1)
- Unity 2021 LTS (최소 버전: 2021.3.18f1)

안정적인 사용을 위해 각 버전의 최신 LTS 패치 사용을 권장합니다.
:::

## 설치 단계

### 1. Registry 설정하기

1. Unity 편집기에서 **Edit → Project Settings**로 이동합니다.
   ![프로젝트 설정 메뉴](/img/docs/guide/world/sdk/install/01.png)

2. Package Manager에서 'Enable Preview Packages' 옵션을 활성화합니다.
   ![프리뷰 패키지 활성화](/img/docs/guide/world/sdk/install/02.png)

3. Registry 정보를 다음과 같이 입력합니다:

   **Name:**
   ```
   backndworld
   ```
   ![Name 입력](/img/docs/guide/world/sdk/install/03.png)

   **URL:**
   ```
   https://registry.npmjs.org
   ```
   ![URL 입력](/img/docs/guide/world/sdk/install/04.png)

   **Scope(s):**
   ```
   com.backnd.world
   ```
   ![Scope 입력](/img/docs/guide/world/sdk/install/05.png)

4. Save 버튼을 클릭하여 설정을 저장합니다.
   ![설정 저장](/img/docs/guide/world/sdk/install/06.png)

5. 정상 입력 예시
   ![프로젝트 설정](/img/docs/guide/world/sdk/project-settings.png)

### 2. 패키지 설치하기

1. **Window → Package Manager**를 선택합니다.
   ![패키지 매니저](/img/docs/guide/world/sdk/install/07.png)

2. 드롭다운 메뉴에서 **My Registries**를 선택합니다.
   ![My Registries 선택](/img/docs/guide/world/sdk/install/08.png)

   :::note
   My Registries가 보이지 않는다면 Registry 설정이 올바르게 되었는지 확인해주세요.
   :::

3. 목록에서 패키지를 찾아 Install 버튼을 클릭합니다.
   ![패키지 설치](/img/docs/guide/world/sdk/install/09.png)

### 3. 초기 설정하기

1. Hierarchy 창에서 **Create Empty**를 클릭하여 새 게임 오브젝트를 생성합니다.
   ![Create Empty](/img/docs/guide/world/sdk/install/10.png)

2. 생성된 오브젝트를 선택한 후 Inspector 창에서 **Add Component**를 클릭하고 **NetworkManager**를 추가합니다.
   ![NetworkManager 추가](/img/docs/guide/world/sdk/install/11.png)
