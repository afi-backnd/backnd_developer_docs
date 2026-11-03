---
sidebar_label: 빌드
description: "빌드"
---

# 빌드

서버에 배포할 파일은 반드시 `backend build` 또는 `backend deploy`로 만들어야 합니다.  
Visual Studio나 Visual Studio Code의 빌드 결과는 내 PC 디버깅용이므로 배포에 사용할 수 없습니다.  

| 빌드 방법 | 용도 | 결과 위치 |
|---|---|---|
| `backend deploy` | 빌드와 배포를 함께 진행 | `.backend/publish.zip` |
| `backend build` | 배포 전에 빌드만 확인 | `.backend/publish.zip` |
| 편집기 F5, `backend debug` | 내 PC에서 실행·디버깅 | `.backend/debug` |

서버는 Linux 환경에서 함수를 실행합니다.  
`backend`는 Windows나 Mac에서 빌드하더라도 서버용(`linux-x64`, Release)으로 빌드하고, 서버가 찾는 이름(`BackendFunction.dll`)으로 고정해 ZIP 파일을 만듭니다.  

자세한 사용법은 [빌드](/sdk-docs/function/cli/build) 문서를 참고해 주세요.  
