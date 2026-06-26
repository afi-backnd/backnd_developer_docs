---
sidebar_label: 유저 정보 조회 V2(서버)
sidebar_position: 1.5
---

# GetUserInfoV2
public BackendReturnObject **GetUserInfoV2**();

## 설명
서버에 저장된 유저의 메타 정보를 받아옵니다.  
V2에서는 기존 API의 리턴값에 그룹 이름, 길드 inDate, 최근 접속 시각이 추가 되었습니다.  

### 유저의 메타 정보
- 닉네임
- 유저 inDate
- gamer_id(콘솔에서 사용되는 유저 고유값)
- 로그인 타입(커스텀, 페더레이션)
- 페더레이션 계정 아이디(구글/애플/페이스북등 연동한 페더레이션 아이디)
- 커스텀 계정 id/pw 찾기용 이메일
- 국가 코드
- 그룹 이름
- 길드 inDate
- 최근 접속 시각

## Example

### 동기
```js
BackendReturnObject bro = Backend.BMember.GetUserInfoV2();
string nickname = bro.GetReturnValuetoJSON()["row"]["nickname"].ToString();
```

### 비동기
```js
Backend.BMember.GetUserInfoV2((callback) =>
{
    string nickname = callback.GetReturnValuetoJSON()["row"]["nickname"].ToString();
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.BMember.GetUserInfoV2, (callback) =>
{
    string nickname = callback.GetReturnValuetToJSON()["row"]["nickname"].ToString();
});
```

## ReturnCase

### Success cases
**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON
성공 시, 로그인한 유저의 정보를 나타냅니다.  
```js
{
    "row": {
        "gamerId":"123456a0-7890-11ab-22cd-33f4567fa89" // 유저의 gamer_id
        "countryCode": "KR", // 국가 코드를 설정하지 않은 경우 null
        "nickname": "테스트유저1", // 닉네임을 설정하지 않은 경우 null
        "inDate": "2020-06-23T05:54:29.743Z", // 유저의 inDate
        "propertyGroup": "USA", // 그룹이 없는 경우 null,
        "guildInDate": null,    // 길드가 없는 경우 null
        "lastLogin": "2025-04-24T02:58:49.730Z",    // 최근 접속한 시각
        "emailForFindPassword": "backend@afidev.com", // 커스텀 계정 id, pw 찾기 용 이메일. 등록하지 않으면 null
        "subscriptionType": "customSignUp", // 커스텀, 페더레이션 타입
        "federationId": null // 구글, 애플, 페이스북 페더레이션 ID. 커스텀 계정은 null
    }
}
```

## Sample Code
```js
public class UserInfo
{
    public string gamerId;
    public string countryCode;
    public string nickname;
    public string inDate;
    public string propertyGroup;
    public string guildInDate;
    public string lastLogin;
    public string emailForFindPassword;
    public string subscriptionType;
    public string federationId;

    public override string ToString()
    {
        return $"gamerId: {gamerId}\n" +
        $"countryCode: {countryCode}\n" +
        $"nickname: {nickname}\n" +
        $"inDate: {inDate}\n" +
        $"propertyGroup: {propertyGroup}\n" +
        $"guildInDate: {guildInDate}\n" +
        $"lastLogin: {lastLogin}\n" +
        $"emailForFindPassword: {emailForFindPassword}\n" +
        $"subscriptionType: {subscriptionType}\n" +
        $"federationId: {federationId}\n";
    }
}
```

```js
public void GetUserInfoTest()
{
    var bro = Backend.BMember.GetUserInfo();

    if(!bro.IsSuccess())
    {
        Debug.LogError("에러가 발생했습니다 : " + bro.ToString());
        return;
    }

    LitJson.JsonData userInfoJson = bro.GetReturnValuetoJSON()["row"];

    UserInfo userInfo = new UserInfo();

    userInfo.gamerId = userInfoJson["gamerId"].ToString();
    userInfo.countryCode = userInfoJson["countryCode"].ToString();
    userInfo.nickname = userInfoJson["nickname"].ToString();
    userInfo.inDate = userInfoJson["inDate"].ToString();
    userInfo.propertyGroup = userInfoJson["propertyGroup"]?.ToString();
    userInfo.guildInDate = userInfoJson["guildInDate"].ToString();
    userInfo.lastLogin = userInfoJson["lastLogin"].ToString();
    userInfo.emailForFindPassword = userInfoJson["emailForFindPassword"]?.ToString();
    userInfo.subscriptionType = userInfoJson["subscriptionType"].ToString();
    userInfo.federationId = userInfoJson["federationId"]?.ToString();

    Debug.Log(userInfo.ToString());
}
```
