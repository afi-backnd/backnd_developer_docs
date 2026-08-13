---
sidebar_label: "[Legacy] 닉네임으로 유저 정보 조회"
description: "GetUserInfoByNickName"
---

# GetUserInfoByNickName

public BackendReturnObject **GetUserInfoByNickName**(string **nickName**);

## 파라미터

| Value    | Type   | Description                 |
| -------- | ------ | --------------------------- |
| nickName | string | 정보를 조회할 유저의 닉네임 |

## 설명

해당 닉네임을 가지고 있는 유저의 유저 정보를 조회합니다.  
조회가능한 유저 정보는 다음과 같습니다.  

- 유저의 닉네임
- 유저의 gamerIndate
- 유저가 마지막으로 접속한 시간
- 유저가 속한 길드 이름

## Example

### 동기

```js
var bro = Backend.Social.GetUserInfoByNickName("nickName");

//example
string gamerIndate = bro.GetReturnValuetoJSON()["row"]["inDate"].ToString();
if(bro.GetReturnValuetoJSON()["row"]["guildName"] != null) // 길드 네임이 등록되어 있지 않을 경우 null로 반환
{
      string guildName = bro.GetReturnValuetoJSON()["row"]["guildName"].ToString();
}

```

### 비동기

```js
Backend.Social.GetUserInfoByNickName("나는야테스트유저", (callback) =>
{
    // 이후 처리

    //example
    string gamerIndate = callback.GetReturnValuetoJSON()["row"]["inDate"].ToString();
    if(callback.GetReturnValuetoJSON()["row"]["guildName"] != null) // 길드 네임이 등록되어 있지 않을 경우 null로 반환
    {
        string guildName = callback.GetReturnValuetoJSON()["row"]["guildName"].ToString();
    }
});

```

### SendQueue

```js
SendQueue.Enqueue(Backend.Social.GetUserInfoByNickName, "나는야테스트유저", (callback) =>
{
    // 이후 처리

    //example
    string gamerIndate = callback.GetReturnValuetoJSON()["row"]["inDate"].ToString();
    if(callback.GetReturnValuetoJSON()["row"]["guildName"] != null) // 길드 네임이 등록되어 있지 않을 경우 null로 반환
    {
        string guildName = callback.GetReturnValuetoJSON()["row"]["guildName"].ToString();
    }
});

```

## ReturnCase

### Success cases

**해당 닉네임을 가진 유저가 존재하는 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**해당 닉네임을 가진 유저가 존재하지 않는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer가 존재하지 않습니다.  

## GetReturnValuetoJSON

```js
{
    "row":
    {
        "nickname":"나는야테스트유저", // 유저의 닉네임
        "inDate":"2021-00-00T00:00:00.000Z", // 유저의 inDate
        "lastLogin":"2021-06-23T02:08:56.235Z", // 유저의 마지막 로그인 시각
        "guildName":"testGuild" // 유저의 길드명(없는 경우 null)
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

     public override string ToString()
     {
         return $"nickname : {nickname}\ninDate : {inDate}\nlastLogin : {lastLogin}\nguildName : {guildName}\n";
     }
 };
```

```js
public void GetUserInfoByNickNameTest()
{
    string userNickname = "닉네임";

    var bro = Backend.Social.GetUserInfoByNickName(userNickname);

    if(!bro.IsSuccess())
        return;

    LitJson.JsonData json = bro.GetReturnValuetoJSON();

    SearchUserItem userInfo = new SearchUserItem();

    userInfo.nickname = json["row"]["nickname"].ToString();
    userInfo.inDate = json["row"]["inDate"].ToString();
    userInfo.lastLogin = json["row"]["lastLogin"].ToString();
    if(json["row"]["guildName"] != null)
    {
        userInfo.guildName = json["row"]["guildName"].ToString();
    }

    Debug.Log(userInfo.ToString());
}
```
