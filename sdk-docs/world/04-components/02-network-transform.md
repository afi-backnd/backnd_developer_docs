---
sidebar_label: NetworkTransform
description: "NetworkTransform"
---

# NetworkTransform

컴포넌트 하나로 오브젝트의 위치, 회전, 크기를 네트워크에 동기화합니다. 보간 처리가 내장되어 있어 부드러운 움직임이 보장됩니다.

## 컴포넌트 종류

NetworkTransform은 두 가지 유형을 제공합니다:
- NetworkTransformReliable: 높은 대역폭, 일반적인 지연 시간
- NetworkTransformUnreliable: 낮은 대역폭, 매우 낮은 지연 시간

:::note
특별히 매우 낮은 지연 시간이 필요한 경우가 아니라면 NetworkTransformReliable 사용을 권장합니다.
:::

## 기본 설정

NetworkTransform 컴포넌트를 사용하려면 반드시 NetworkIdentity 컴포넌트가 필요합니다.

## 동기화 설정

Inspector에서 다음 옵션들을 설정하여 게임 오브젝트의 동기화 방식을 제어할 수 있습니다:

### Selective Sync (선택적 동기화)
게임 오브젝트의 Transform 속성 중 어떤 요소를 네트워크로 동기화할지 선택합니다:

- Sync Position
  - true: 게임 오브젝트의 위치가 네트워크 상에서 동기화됩니다
  - false: 위치 동기화가 비활성화되며, 로컬에서의 위치 변경이 다른 클라이언트에 전송되지 않습니다
  
- Sync Rotation
  - true: 게임 오브젝트의 회전이 네트워크 상에서 동기화됩니다
  - false: 회전 동기화가 비활성화되며, 로컬에서의 회전 변경이 다른 클라이언트에 전송되지 않습니다
  
- Sync Scale
  - true: 게임 오브젝트의 크기가 네트워크 상에서 동기화됩니다
  - false: 크기 동기화가 비활성화되며, 로컬에서의 크기 변경이 다른 클라이언트에 전송되지 않습니다

### Interpolation (보간)
네트워크로 수신된 Transform 값들 사이의 부드러운 전환을 제어합니다:

- Interpolate Position
  - true: 위치 변경이 부드럽게 보간되어 표시됩니다
  - false: 위치 변경이 즉시 적용됩니다
  
- Interpolate Rotation
  - true: 회전 변경이 부드럽게 보간되어 표시됩니다
  - false: 회전 변경이 즉시 적용됩니다
  
- Interpolate Scale
  - true: 크기 변경이 부드럽게 보간되어 표시됩니다
  - false: 크기 변경이 즉시 적용됩니다

:::note
- Selective Sync 설정은 런타임 중에 변경하지 않는 것이 좋습니다
- 일반적으로 Position과 Rotation은 동기화하고, Scale은 필요한 경우에만 활성화합니다
- 부드러운 움직임을 위해 Position과 Rotation의 Interpolation은 활성화하는 것을 권장합니다
:::

### Coordinate Space (좌표계)
트랜스폼 동기화의 기준이 되는 좌표계를 설정합니다:

- Local
  - 부모 오브젝트를 기준으로 한 상대적인 좌표계를 사용합니다
  - 오브젝트가 다른 오브젝트의 자식일 때 유용합니다
  - 부모 오브젝트의 움직임에 따라 상대적으로 동기화됩니다

- World
  - 월드 좌표계를 기준으로 절대적인 위치를 사용합니다
  - 독립적으로 움직이는 오브젝트에 적합합니다
  - 부모 오브젝트와 관계없이 절대적인 위치로 동기화됩니다
