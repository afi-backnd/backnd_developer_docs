---
sidebar_label: "Steam 로그인 인증 예제"
---

# Steamworks.NET 사용

:::info Flow of the Process

1. Steamworks 가입 후 appID와 Web API Key 발급
2. 유니티에 Steamworks.NET 추가
3. 유니티 프로젝트 폴더 내 steam_appid.txt에 발급받은 appID를 입력
4. GetAuthSessionTicket함수를 호출해 세션 티켓 받아오기
5. 발급받은 세션 티켓을 사용해 통해 뒤끝 서버에 로그인  
:::

:::danger 주의

- 해당 문서는 Steamworks.NET의 Release 2025.161.0 버전을 기반으로 예제 코드가 작성되었습니다.
- Release 2025.161.0 보다 낮은 버전에서는 GetAuthSessionTicket함수의 파라미터가 차이가 있으니 예제 코드를 참고해 주세요.
- Steamworks.NET을 추가할 시, 스팀 이외의 플랫폼에서 실행할 수 없습니다.
- 스팀 이외의 플랫폼에서 실행 시 자동종료 후 스팀 플랫폼을 통해 재실행됩니다.
- 자세한 사항은 <a href="https://steamworks.github.io/" target="_blank">Steamworks.NET 개발자 문서</a>를 참고해 주세요.  
:::

## 초기 설정

### 1. 스팀웍스에 애플리케이션 등록

1. [스팀웍스 페이지](https://partner.steamgames.com/)에 접속하여, 앱 성성 후 **대시보드 -> 모든 애플리케이션**을 클릭하여 이동합니다.
2. Steamworks 관리자를 클릭해 **Steamworks 관리자 > 앱 관리자 > (프로젝트명)에서 SteamPipe > 빌드**로 이동합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_01.png" />

3. **여기**를 클릭해 앱을 업로드 하고, **디포를 빌드로 확정**에서 앱을 **default**로 설정 후 **commit**을 클릭합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_02.png" />  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_03.png" />  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_04.png" />

### 2. appID 발급

**Steamworks 관리자 > 앱 관리자 > (프로젝트명)** 페이지의 괄호 안의 숫자가 appID를 가리키며, 앱의 고유번호로써 사용됩니다.  
<img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_05.png" />

### 3. Web API Key 발급 및 앱 등록

**Web API Key 발급을 위해서는 우선 그룹을 생성해야합니다**

1. **Steamworks > 사용자 & 사용권한 > 그룹 관리**로 이동하여, **새로운 그룹 만들기**를 클릭합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_06.png" />

2. 그룹의 이름을 지정하고 **그룹 만들기**를 클릭합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_07.png" />

3. 그룹 생성에 성공했다면 그룹 페이지로 이동되며, 그룹 페이지에서 **WebAPI 키 생성**을 클릭하여 키를 생성합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_08.png" />  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_09.png" />

4. 생성한 Web API 키를 사용하기 위해 그룹에 앱을 등록합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_10.png" />  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_11.png" />

### 4. 뒤끝 콘솔에 스팀 계정 인증 정보 등록

위의 과정에서 발급받은 **appID**와 **Web API Key**를 **뒤끝 콘솔 > 인증 정보 > 스팀 계정 인증 정보**에 등록합니다.  
<img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_12.png" />

## 사용자 초대

**앱을 출시하기 전 테스트를 위해서는 사용자를 그룹에 초대하고, 권한을 부여해줘야 합니다.**

### 1. 사용자 초대 메일 발송 및 권한 설정

1. **Steamworks > 사용자 & 사용권한 > 사용자 관리**로 이동하여, **사용자 초대**를 클릭합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_13.png" />

2. 사용자를 초대하기 위한 이메일 주소와 초대할 사용자의 권한을 설정한 후 **사용자 초대**를 클릭합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_14.png" />

3. 초대를 성공적으로 완료했다면, 작성한 이메일로 초대 메일이 발송됩니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_15.png" />  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_16.png" />  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_17.png" />

4. 사용자가 초대를 수락하면 **확인을 기다리는 초대**에서 사용자의 요청을 수락할 수 있으며, **수락**을 클릭할 시 사용자 초대가 완료됩니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_18.png" />

### 2. 그룹에 추가

1. **Steamworks > 사용자 & 사용권한 > 그룹 관리**에서 위에서 생성한 그룹 페이지로 이동합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_19.png" />

2. **회원 목록 > 사용자 추가**를 클릭하여 그룹에 사용자를 추가합니다.  
   <img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_20.png" />

## 유니티 설정

### 1. 스팀 SDK 설치

[Steamworks.NET](https://github.com/rlabrecque/Steamworks.NET/releases)을 설치한 후 유니티 프로젝트로 임포트 합니다.  
<img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_21.png" />

### 2. steam_appid.txt 수정

유니티 프로젝트 폴더 안에 있는 steam_appid.txt파일을 열고, 스팀 appID를 입력합니다.  
아무것도 입력하지 않았다면 480이 입력되어있습니다.  
<img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_22.png" />

### 3. SteamManager스크립트를 씬에 추가

**Assets > Scripts > Steamworks.NET > SteamManager.cs**를 Hierarchy의 빈 오브젝스를 생성해 컴포넌트로 추가합니다.  
<img src="https://developer.thebackend.io/static/img/unity/SteamLogin/SteamSDK_23.png" />

## 스팀 페더레이션을 통한 뒤끝 서버 로그인

---

### AuthorizeFederation

public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**);  
public BackendReturnObject **AuthorizeFederation**(string **federationToken**, FederationType **type**, string **ect**);

### 파라미터

| Value           | Type           | Description                                                 |
| --------------- | -------------- | ----------------------------------------------------------- |
| federationToken | string         | 각 로그인 플러그인을 통해 생성된 token 값                   |
| federationType  | FederationType | 페더레이션의 종류.(FederationType.Steam)                    |
| ect             | string         | (Optional) 부가적으로 나오는 정보들 중에 저장하고 싶은 정보 |

### 설명

스팀의 세션티켓 이용하여 회원가입/로그인을 시도합니다.

**OnGetAuthSessionTicketResponse()** 에서 **세션티켓**값을 반환받아 게임에 회원가입/로그인할 수 있습니다.  
리턴되는 스팀의 세션티켓 값은 다음과 같습니다.

```js
140000006052bf05a058e52b0d22cb36010010012f435d65180000000100000002000000118c26a42d964b02eec9880001000000c400000044000000040000000d22cb3601001001702c130020c34f341600080a0000000046565c65c60578650100aea806000300626713000000984d29000000a24d290000000000baa2c46a231b8b9ff2def6c5e5e30db4ae8858aaa764a240c8a061203d59ff0f54adaff1920ef8dad9b04aafb04ade640138f3ccf7187e6bf44f79cc60355d5d457ee5a6d1a304dc73ed0861e4a3553b9ad037e17782a1a3474443d18c4db0f830afb18d931a9ba0040e8569b862c63bf81d30854783d77cc828c3d8160ca481
```

### 필요한 namespace

```js
using Steamworks;
```

### 스팀 세션 티켓 받아오는 코드 Example

Start 함수에서 GetAuthSessionTicket함수를 호출할 경우, 호출이 종료될 때 OnGetAuthSessionTicketResponse함수에서 값을 반환하게 됩니다.

```js
    private byte[] m_Ticket;
    private uint m_pcbTicket;
    private HAuthTicket m_HAuthTicket;

    string sessionTicket = string.Empty;

    protected Callback<GetAuthSessionTicketResponse_t> m_GetAuthSessionTicketResponse;

    void OnGetAuthSessionTicketResponse(GetAuthSessionTicketResponse_t pCallback)
    {
        //Resize to buffer of 1024
        System.Array.Resize(ref m_Ticket, (int)m_pcbTicket);

        //format as Hex
        System.Text.StringBuilder sb = new System.Text.StringBuilder();

        foreach (byte b in m_Ticket) sb.AppendFormat("{0:x2}", b);

        sessionTicket = sb.ToString();
        Debug.Log("Hex encoded ticket: " + sb.ToString());
    }

    void Start()
    {
        Backend.Initialize();

        if (SteamManager.Initialized)
        {
            m_GetAuthSessionTicketResponse = Callback<GetAuthSessionTicketResponse_t>.Create(OnGetAuthSessionTicketResponse);

            m_Ticket = new byte[1024];
            SteamNetworkingIdentity identity = new SteamNetworkingIdentity();
            identity.SetSteamID(SteamUser.GetSteamID());            
            m_HAuthTicket = SteamUser.GetAuthSessionTicket(m_Ticket, 1024, out m_pcbTicket, ref identity);
            //구 버전 Steamworks.NET의 경우 (ex : Release 20.x.0)
            //m_HAuthTicket = SteamUser.GetAuthSessionTicket(m_Ticket, 1024, out m_pcbTicket);
        }
    }
```

### 일반 코드 Example

```js
    BackendReturnObject bro = Backend.BMember.AuthorizeFederation(sessionTicket, FederationType.Steam);

    if(bro.IsSuccess())
    {
        Debug.Log("Steam 로그인 성공");
        //성공 처리
    }
    else
    {
        Debug.LogError("Steam 로그인 실패");
        //실패 처리
    }
```

### 기타 스팀 함수

```js
Debug.Log("스팀 아이디 : " + SteamUser.GetSteamID()); // 고유번호 - 숫자 조합
Debug.Log("사용자 닉네임 : " + SteamFriends.GetPersonaName()); // 닉네임 - 닉네임 (ex : Backend01 )
Debug.Log("사용자 국가 정보 : " + SteamUtils.GetIPCountry()); // 유저 국가 정보 - KR
```

## ReturnCase

### Success cases

**로그인에 성공한 경우**  
statusCode : 200  
message : Success

**신규 회원가입에 성공한 경우**  
statusCode : 201  
message : Success

### Error cases

**차단당한 계정일 경우**  
statusCode : 403  
errorCode : 콘솔에서 입력한 차단된 사유  
message : Forbidden blocked user, 금지된 blocked user
