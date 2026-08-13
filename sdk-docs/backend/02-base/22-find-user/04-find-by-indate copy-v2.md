---
sidebar_label: "유저 inDate로 유저 정보 조회 V2"
sidebar_position: "0.75"
description: "GetUserInfoByInDateV2"
---

# GetUserInfoByInDateV2

public BackendReturnObject **GetUserInfoByInDateV2**(string **gamerInDate**);

## 파라미터

| Value       | Type   | Description                      |
| ----------- | ------ | -------------------------------- |
| gamerInDate | string | 정보를 조회할 유저의 gamerInDate |

## 설명
해당 gamerInDate를 가지고 있는 유저의 유저 정보를 조회합니다.  
V2에서는 기존 API의 리턴값에 국가 코드, 그룹 이름이 추가 되었습니다.  

### 유저의 메타 정보
- 유저의 닉네임
- 유저의 gamerIndate
- 유저가 마지막으로 접속한 시간
- 유저가 속한 길드 이름
- 국가 코드
- 그룹 이름

## Example

### 동기

```js
var bro = Backend.Social.GetUserInfoByInDate("2021-00-00T00:00:00.000Z");

//example
string nickname = bro.GetReturnValuetoJSON()["row"]["nickname"].ToString();
if(bro.GetReturnValuetoJSON()["row"]["guildName"] != null) // 길드 네임이 등록되어 있지 않을 경우 null로 반환
{
      string guildName = callback.GetReturnValuetoJSON()["row"]["guildName"].ToString();
}
```

### 비동기

```js
Backend.Social.GetUserInfoByInDate("2021-00-00T00:00:00.000Z", (callback) =>
{
    // 이후 처리

    //example
    string nickname = callback.GetReturnValuetoJSON()["row"]["nickname"].ToString();
    if(callback.GetReturnValuetoJSON()["row"]["guildName"] != null) // 길드 네임이 등록되어 있지 않을 경우 null로 반환
    {
          string guildName = callback.GetReturnValuetoJSON()["row"]["guildName"].ToString();
    }
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Social.GetUserInfoByInDate, "2021-00-00T00:00:00.000Z", (callback) =>
{
    // 이후 처리

    //example
    string nickname = callback.GetReturnValuetoJSON()["row"]["nickname"].ToString();
    if(callback.GetReturnValuetoJSON()["row"]["guildName"] != null) // 길드 네임이 등록되어 있지 않을 경우 null로 반환
    {
          string guildName = callback.GetReturnValuetoJSON()["row"]["guildName"].ToString();
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
public void GetUserInfoByInDateTest()
{
    string userNickname = "2021-11-24T02:44:14.638Z";

    var bro = Backend.Social.GetUserInfoByInDate(userNickname);

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData json = bro.GetReturnValuetoJSON();

    SearchUserItem userInfo = new SearchUserItem();

    userInfo.nickname = json["row"]["nickname"].ToString();
    userInfo.inDate = json["row"]["inDate"].ToString();
    userInfo.lastLogin = json["row"]["lastLogin"].ToString();
    userInfo.guildName = json["row"]["guildName"]?.ToString();    
    userInfo.countryCode = json["row"]["countryCode"]?.ToString();
    userInfo.propertyGroup = json["row"]["propertyGroup"]?.ToString();
    
    Debug.Log(userInfo.ToString());
}
```
