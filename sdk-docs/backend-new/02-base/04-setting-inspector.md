---
description: "뒤끝 SDK Inspector 설정"
---

# 뒤끝 SDK Inspector 설정

뒤끝 인스펙터 창에서는 뒤끝 SDK를 사용하기 위한 다양한 값을 설정할 수 있습니다.  
아래는 각 값에 대한 자세한 설명입니다.  

### Server Settings
<img src="https://developer.thebackend.io/static/img/unity/guide/inspector/sdk-server-setting.png" />

| 변수 | 설명 |
|--- |--- |
| **Client App ID** | 게임을 구분하는 게임 고유 아이디 |
| **Signature Key** | 게임 데이터를 서버와 주고받을 시 데이터를 암호화할 때 필요한 키 |
| **Function Auth Key** | 뒤끝펑션을 사용할 때 인증을 위한 키 |
| **Package Name** | Unity Project의 packageName입니다. |
| **Is All Platform** | 해당 설정값은 Android 혹은 iOS 기기 이외의 다른 OS 허용 여부를 결정하는 값입니다.  해당 값이 체크되어 있지 않은 경우, Android 혹은 iOS 이외의 OS에서 접속하는 경우 서버에서 오류를 리턴합니다. |
| **Send Log Report** | 서버로 요청을 보낼 때 서버에 추가로 로그를 저장할지 결정하는 값입니다.해당 값이 체크되어 있으면 요청을 보낼 때 기존에 남기는 로그이외에 추가 로그를 남기게 됩니다.  이는 추후 오류사항을 추적하거나 뒤끝을 개선하는 데 사용됩니다.  체크 유무에 따라 응답속도에 큰 차이는 없으나 체크하는 경우 응답속도가 소폭 느려질 수 있습니다. |
| **Rate Count Active** | 클라이언트가 실행되는 동안 뒤끝 함수를 카테고리 별로 얼마나 호출하였는지 기록합니다. Backend.RateCounter.GetRateCount()를 통해 확인할 수 있습니다. |
| **Time Out Sec** | 서버로 요청을 보냈을 때 설정한 시간(초) 동안 응답이 오지 않았을 경우 타임아웃 에러로 처리할 시간을 결정합니다.(default: 100초) |

### Error Handling
<img src="https://developer.thebackend.io/static/img/unity/guide/inspector/sdk-error-handling.png" />


### Device Id
<img src="https://developer.thebackend.io/static/img/unity/guide/inspector/sdk-device-id.png" />

| 변수 | 설명 |
|--- |--- |
| Init Fail When Device Id Unknown | DeviceId 없을 시 초기화 실패 처리할지에 대한 여부 |

### Auto Location
> <img src="https://developer.thebackend.io/static/img/unity/guide/inspector/sdk-auto-location.png"/>

| 변수 | 설명 |
|--- |--- |
| Auto Load Location Properties | 국가 정보 갱신 함수를 초기화 시에 비동기로 자동적으로 호출할 것인지 |
