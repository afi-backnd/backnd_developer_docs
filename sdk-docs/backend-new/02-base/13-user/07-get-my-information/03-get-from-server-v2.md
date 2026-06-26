---
sidebar_label: 유저 정보 조회(서버)
sidebar_position: 1.5
---

# GetUserInfo
public Task&lt;GetUserInfoResult&gt; **GetUserInfoAsync**();

## 설명
서버에 저장된 유저의 메타 정보를 받아옵니다.  

### 유저의 메타 정보
- 닉네임
- 유저 inDate
- gamer_id(콘솔에서 사용되는 유저 고유값)
- 로그인 타입(커스텀, 페더레이션)
- 외부 계정 아이디(구글/애플/페이스북등 연동한 외부 계정 아이디)
- 커스텀 계정 id/pw 찾기용 이메일
- 국가 코드
- 그룹 이름
- 길드 inDate
- 최근 접속 시각

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.GetUserInfoAsync();
var nickname = reqResult.GetInfo().Nickname;
```

### Callback 방식
```js
BackndAuth.Instance.GetUserInfoAsync((callback) =>
{
    var nickname = reqResult.GetInfo().Nickname;
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
        "federationId": null // 구글, 애플, 페이스북 계정 ID. 커스텀 계정은 null
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
public async Task GetUserInfoTest()
{
    var reqResult = await BackndAuth.Instance.GetUserInfoAsync();
    if (!reqResult.IsSuccess())
    {
        Debug.LogError("에러가 발생했습니다 : " + reqResult.ToString());
        return;
    }

    var info = reqResult.GetInfo();

    UserInfo userInfo = new UserInfo();
    userInfo.gamerId = info.GamerId;
    userInfo.countryCode = info.CountryCode;
    userInfo.nickname = info.Nickname;
    userInfo.inDate = info.InDate;
    userInfo.propertyGroup = info.PropertyGroup;
    userInfo.guildInDate = info.GuildInDate;
    userInfo.lastLogin = info.LastLogin;
    userInfo.emailForFindPassword = info.EmailForFindPassword;
    userInfo.subscriptionType = info.SubscriptionType;
    userInfo.federationId = info.FederationId;

    Debug.Log(userInfo.ToString());
}
```
