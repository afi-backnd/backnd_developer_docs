---
sidebar_label: "업데이트 내역"
---

# 업데이트 상세 내역

:::caution 뒤끝펑션 0.1.2 버전 비호환 문제
SDK 5.9.0 버전에서 0.1.2 이하(2021-12-28 이전에 배포된 펑션) 버전의 뒤끝펑션을 호출할 경우,  
**펑션 내부에서 호출하는 함수에 에러가 발생하여 정상적으로 작동하지 않게 됩니다.**
  
꼭 SDK를 업그레이드하기 전에 **펑션을 0.2.0 버전으로 업그레이드**하여 사용해주세요.  
:::

:::danger SDK 5.6.0 이하 업데이트 후 410 에러 발생 문제
SDK 5.6.0 이하 버전에서 현재 버전으로 업데이트를 할 경우, 모든 뒤끝 함수 호출에서 **410 GoneResourceException** 에러가 발생할 수 있습니다.  
재로그인 시, 해당 에러가 이후 발생하지 않게되므로 아래와 같은 로그인 함수를 다시 호출할 수 있도록 구성해주세요.  


  * BackndAuth.Instance.SignInCustom
  * BackndAuth.Instance.SignInGuest
  * BackndAuth.Instance.SignInWithProvider  
  * BackndAuth.Instance.SignInWithBackndToken
  * BackndAuth.Instance.RefreshBackndToken
:::

:::info SDK 5.8.0 403 Forbidden 로직 개편 안내
SDK 5.8.0 버전에서는 과도한 요청으로 발생되는 403 Forbidden 에러 발생 시, 이후부터는 서버로 보내는 송신을 로컬에서 5분 30초동안 금지하며 해당 시간 내 함수 호출 시 다음과 같은 에러를 리턴합니다.  

statusCode : 403  
errorCode : Forbidden  
message : 403 Forbidden by Local  
  
만약 403 에러 처리에 기존 message를 이용할 경우에는 errorCode를 이용하거나 <a href="https://docs.thebackend.io/sdk-docs/backend/base/common-errors/functions/too-many-request/">IsTooManyRequestError</a> 함수를 이용해주세요.  
:::

:::danger SDK 5.11.0 ~ 5.11.3 압축형데이터 이용 불가 안내
SDK 5.11.0에서 5.11.3의 경우 압축형 데이터를 이용할 경우, 데이터 불러오기 시, inDate에 뒷자리에 0이 붙을 경우, inDate값이 변경되는 치명적인 오류가 존재합니다.  
압축형 데이터를 이용하고자 할 경우에는 꼭 5.11.4 이상의 SDK로 진행해주시기 바랍니다.  
:::

---
# 6.0.1
### 네임 스페이스 변경
- 기존 `BackEnd` 에서 `BACKND.Base`로 변경 되었습니다.

### 접근 객체 변경
- 기존 `Backend` 에서 각 기능별 인스턴스 이름으로 접근하도록 변경 되었습니다.
  * ex1) `Backend.Leaderboard.User`  ⇒  `BackndLeaderboard.User`
  * ex2) `Backend.BMember` => `BackndAuth.Instance`
- 기존 `Backend` 가 가진 static 접근 기능은 `BackndBase` 클래스가 대신합니다.

### 함수명 변경
- 일관성이 없거나 API명에 잘 사용되지 않는 함수들의 이름이 변경되었습니다.

### 동기 함수 제거 및 Task 기반 비동기 함수 추가
- 동기식 함수를 제거하고 동기식처럼 대기하면서도 비동기로 동작가능한 Task 함수를 추가했습니다.
- Task 함수들은 함수명 뒤에 ~Async 접미사가 있습니다.

### 서버 요청 응답 클래스 변경
- 기존 `BackendReturnObject` 대신 `RequestResult`로 변경되었습니다.
- `RequestResult`는 전달 받은 응답 데이터(`ReturnValue`)를 내부적으로 JObject 객체로 파싱하여 필요값을 가져갈 수 있도록 제공합니다.
- 언마샬이 필요한 경우에는 자동으로 언마샬을 적용하여 순수 값만으로 구성된 JObject를 제공합니다.
- 자세한 내용은 [뒤끝필수지식 - RequestResult](/sdk-docs/backend/base/knowhow/object-for-return/start)를 참조하세요

### Json Parser 라이브러리 일원화
- 기존에는 Litjson과 Newtonsoft json이 혼재되어 있었으나 Litjson을 제거하고 Newtonsoft json을 사용하도록 변경하였습니다.
- 따라서 기존 리턴값에서 제공하던 Litjson 파싱은 더이상 제공하지 않습니다.
- LitJson 사용하시려면 별도로 라이브러리를 설치하고 `RequestResult` 의 ReturnValue값을 전달하면 litjson용 data를 구성해서 사용할 수 있습니다.

### SendQueue 기능 제거
- 기존의 SendQueue 기능이 제거되었습니다.
- Task 기반 비동기 함수가 추가되었으므로 async/await를 활용하여 SendQueue처럼 순차처리가 가능하도록 구성할 수 있습니다.

### BackndLegacy.dll 분리
- 신규 기능이 제공되어 구버전이 된 기능들을 BackndLegacy.dll에 모아 별도 분리하여 제공합니다.
- 대상 기능은 다음과 같습니다.
  * `GameData` : `BackndUserData`로 대체 가능합니다.
  * `Chart` : CDN으로 동작하는 `BackndGameTable`로 대체 가능합니다.
  * `Probability` : CDN으로 동작하는 `BackndRate` 로 대체 가능합니다.
  * `Ranking` : `BackndLeaderboard`로 대체 가능합니다.
- 사용법
  * 네임스페이스 : `BACKND.Base.Legacy`
  * `BackndLegacy.[기능이름]` 으로 접근 가능
