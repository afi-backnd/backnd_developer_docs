---
sidebar_label: "유저 inDate로 유저 정보 조회"
description: "유저 inDate로 유저 정보 조회"
sidebar_position: 0.75
---

# GetUserInfoByInDate

public Task< GetUserInfoByResult > **GetUserInfoByInDateAsync**(string **userInDate**);

## 파라미터

| Value       | Type   | Description                      |
| ----------- | ------ | -------------------------------- |
| userInDate | string | 정보를 조회할 유저의 userInDate |

## 설명
해당 userInDate를 가지고 있는 유저의 유저 정보를 조회합니다.  

### 유저의 메타 정보
- 유저의 닉네임
- 유저의 gamerIndate
- 유저가 마지막으로 접속한 시간
- 유저가 속한 길드 이름
- 국가 코드
- 그룹 이름

## Example

### Task 방식

```js
var reqResult = await BackndSocial.Instance.GetUserInfoByInDateAsync("2021-00-00T00:00:00.000Z");

//example
var info = reqResult.GetInfo();
string gamerIndate = info.InDate;
if (!string.IsNullOrEmpty(info.GuildName))
{
    string guildName = info.GuildName;
}
```

### Callback 방식

```js
BackndSocial.Instance.GetUserInfoByInDate("2021-00-00T00:00:00.000Z", (callback) =>
{
    // 이후 처리

    //example
    var info = reqResult.GetInfo();
    string gamerIndate = info.InDate;
    if (!string.IsNullOrEmpty(info.GuildName))
    {
        string guildName = info.GuildName;
    }
});
```

## ReturnCase

### Success cases

**해당 inDate을 가진 유저가 존재하는 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**해당 inDate을 가진 유저가 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer가 존재하지 않습니다.  

## GetReturnValuetoJSON

```js
{
    "row":
    {
        "nickname":"나는야테스트유저", // 유저의 닉네임(없는 경우 null)
        "inDate":"2021-00-00T00:00:00.000Z", // 유저의 inDate
        "lastLogin":"2021-06-23T02:08:56.235Z", // 유저의 마지막 로그인 시각
        "guildName":"testGuild" // 유저의 길드명(없는 경우 null)
        "countryCode": "KR",    // 국가 코드를 설정하지 않은 경우 null
        "propertyGroup": "USA"  // 그룹이 없는 경우 null,
    }
}
```

## Sample Code

```js
  public class SearchUserItem
  {
      public string nickname;
      public string inDate;
      public string lastLogin;
      public string guildName;
      public string countryCode;
      public string propertyGroup;

      public override string ToString()
      {
        return $"nickname: {nickname}\n" +
        $"inDate: {inDate}\n" +
        $"lastLogin: {lastLogin}\n" +
        $"guildName: {guildName}\n" +
        $"countryCode: {countryCode}\n" +
        $"propertyGroup: {propertyGroup}\n";        
      }
  };
```

```js
public async Task GetUserInfoByInDateTest()
{
    string userInDate = "2021-11-24T02:44:14.638Z";

    var reqResult = await BackndSocial.Instance.GetUserInfoByInDateAsync(userInDate);
    if (!reqResult.IsSuccess())
        return;

    var info = reqResult.GetInfo();

    SearchUserItem userInfo = new SearchUserItem();
    userInfo.nickname = info.Nickname;
    userInfo.inDate = info.InDate;
    userInfo.lastLogin = info.LastLogin;
    userInfo.guildName = info.GuildName;
    userInfo.countryCode = info.CountryCode;
    userInfo.propertyGroup = info.PropertyGroup;

    Debug.Log(userInfo.ToString());
}
```
