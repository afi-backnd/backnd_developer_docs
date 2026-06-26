---
sidebar_label: 빌드 & 배포
description: "빌드 & 배포"
---

# 빌드 & 배포

Unity에서 Linux 서버 빌드 후 콘솔에 업로드하면 서버가 배포되고 자동 스케일링됩니다. 서버 인프라 구축 없이 게임 개발에 집중할 수 있습니다.

## 빌드

### Unity 빌드 설정

1. Unity 편집기에서 **File > Build Settings**로 이동합니다.
![빌드 설정 메뉴](/img/docs/guide/world/sdk/build/01.png)

2. **Platform**에서 **Dedicated Server**를 선택합니다.
![서버 플랫폼 선택](/img/docs/guide/world/sdk/build/02.png)

3. **Target Platform**을 **Linux**로 선택합니다.
![Linux 선택](/img/docs/guide/world/sdk/build/03.png)

4. **Switch Platform** 버튼을 클릭하여 플랫폼을 변경합니다.
![Switch Platform 버튼](/img/docs/guide/world/sdk/build/04.png)

5. 우측 하단의 **Build** 버튼을 클릭합니다.
![Build 버튼](/img/docs/guide/world/sdk/build/05.png)

6. 빌드 파일의 이름을 입력하고 저장 위치를 선택합니다.
![빌드 파일 저장](/img/docs/guide/world/sdk/build/06.png)

## 배포

### 빌드 파일 준비

1. 빌드가 완료된 폴더로 이동하여 생성된 파일들의 상위 폴더를 선택합니다.
![상위 폴더 선택](/img/docs/guide/world/sdk/build/07.png)

2. 선택한 폴더를 zip 또는 gz 형식으로 압축합니다.
![폴더 압축](/img/docs/guide/world/sdk/build/08.png)

### 서버 업로드

1. 콘솔 페이지에서 **월드 서버**를 클릭합니다.
![월드 메뉴 선택](/img/docs/guide/world/sdk/build/09.png)

2. 월드 서버 목록에서 배포 해야 할 월드 서버를 선택합니다.
![월드 서버 메뉴](/img/docs/guide/world/sdk/build/10.png)

3. **빌드 파일 업로드** 버튼을 클릭하여 압축한 빌드 파일을 업로드합니다.
![빌드 파일 업로드](/img/docs/guide/world/sdk/build/11.png)

## 빌드 파일 요구사항

### 압축 파일 형식
- 지원: `.zip`, `.gz`
- 미지원: `.rar`, `.7z` 등

### 파일명 규칙
압축 파일명과 빌드 파일명이 동일해야 합니다:
```
✅ 올바른 예시:
MyGame.zip
└── MyGame.x86_64
└── MyGame_Data/

❌ 잘못된 예시:
MyGame_v1.zip
└── MyGame.x86_64  // 파일명 불일치
```

### 파일 구조
실행 파일은 최상위 디렉토리에 위치해야 합니다:
```
✅ 올바른 구조:
MyGame.zip
├── MyGame.x86_64       // 최상위 위치
├── MyGame_Data/
│   ├── Resources/
│   ├── Managed/
│   └── ...

❌ 잘못된 구조:
MyGame.zip
├── Build/
│   └── MyGame.x86_64   // 하위 디렉토리에 위치
└── MyGame_Data/
```

:::note
빌드 파일 업로드 시 다음 사항을 반드시 확인해주세요:
- 압축 파일과 실행 파일의 이름이 반드시 일치해야 합니다.
- 실행 파일은 반드시 최상위 디렉토리에 있어야 합니다.
- zip 또는 gz 형식만 지원됩니다.
:::
