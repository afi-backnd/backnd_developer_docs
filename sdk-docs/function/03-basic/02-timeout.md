---
sidebar_label: 타임아웃
description: "타임아웃"
---

# 타임아웃

뒤끝펑션에는 실행 제한 시간이 있습니다. 함수 1회 실행당 제한 시간은 **10000ms(10초)**입니다.  

:::warning
로직을 실행하는 데 10초 이상 걸리면 서버에서 처리하지 않고 오류를 반환합니다. 실행 시간을 확인해 로직을 최적화해 주세요.  
:::

## 내 PC에서 실행할 때 제한 시간

`backend debug`는 서버와 같은 **10초** 제한으로 실행합니다. 설정 파일을 고치지 않아도 됩니다. 제한 시간을 직접 정하려면 `--timeout`으로 초 단위를 지정합니다. `0`이면 제한이 없습니다.  

| 실행 방법 | 제한 시간 |
|---|---|
| `backend debug` | 10초 |
| `backend debug --timeout 5` | 5초 |
| `backend debug --timeout 0` | 없음 |
| 편집기의 F5 디버깅 | 없음 |

서버의 실행 환경 차이나 Cold Start로 실행 시간이 늘어나도 여유가 있도록, 서버보다 짧은 5초로 확인해 두는 것을 권장합니다.  

```powershell
backend debug --timeout 5
```

결과의 `timeoutSeconds`와 `timeoutSource`에서 적용된 제한 시간을 확인할 수 있습니다. 기본값은 `"timeoutSource": "default"`, `--timeout`을 지정하면 `"flag"`입니다.  

:::info 편집기에서 디버깅할 때
Visual Studio와 Visual Studio Code의 F5 디버깅은 중단점에서 멈춰 있을 수 있도록 제한 시간 없이 실행합니다.  
제한 시간 안에 끝나는지는 `backend debug`로 확인해 주세요.  
:::

`--user`(테스트 계정으로 실행)를 사용할 때 로그인 정보를 가져오는 시간은 제한 시간에 포함되지 않습니다. 함수를 실행하는 시간만 계산합니다.  

### 제한 시간을 넘은 경우

`backend debug` 결과가 `FUNCTION_EXECUTION_FAILED`로 끝나고, JSON 위에 아래 메시지가 표시됩니다.  

```text
EXECUTION_CANCELLED: Function execution was cancelled or exceeded its timeout.
```

JSON의 `error.details`에는 `"hostErrorCode": "EXECUTION_CANCELLED"`가 표시됩니다.  

### 예전 debugConfig.json의 timeoutEnable

1.0.2 이하 버전에서 만든 프로젝트의 `debugConfig.json`에 `timeoutEnable` 항목이 있어도 `backend debug`는 이 파일을 읽지 않으므로 제한 시간에 영향을 주지 않습니다.  
`--config debugConfig.json`으로 예전 방식 그대로 실행할 때만 `timeoutEnable`을 사용합니다. 이때 `--timeout`을 함께 지정하면 `--timeout`이 우선합니다.  

| `timeoutEnable` | 제한 시간 |
|---|---|
| `true` 또는 `"true"` | 5초 |
| `false` 또는 `"false"` | 없음 |
| 생략 | 10초 |

빈 값(`""`)이나 그 밖의 값을 넣으면 `CONFIG_INVALID` 오류가 납니다. 예전 방식은 [로컬 실행](/sdk-docs/function/cli/debug#예전-debugconfigjson으로-실행하기) 문서를 참고해 주세요.  

## 서버 배포 후 타임아웃

개발 PC와 서버의 실행 환경이 다르기 때문에, 내 PC에서는 제한 시간 안에 끝났더라도 서버에서는 시간 초과가 발생할 수 있습니다. 서버에 배포한 뒤에도 정상적으로 실행되는지 반드시 확인해 주세요.  
[Cold Start](/sdk-docs/function/basic/respone-time#cold-start) 상태에서 호출될 때도 시간 초과가 발생할 수 있습니다. 여러 서드파티 라이브러리를 사용해 프로그램 자체가 크면, 처음 호출될 때 Cold Start에 걸리는 시간 때문에 제한 시간을 넘을 수 있습니다. 이 경우 사용하는 라이브러리를 바꾸거나 로직을 변경해야 합니다.  
