---
sidebar_label: 캐릭터 리스트 불러오기
---

# GetCharacterList
public Task&lt;RequestResult&gt; **GetCharacterListAsync**();  
public Task&lt;RequestResult&gt; **GetCharacterListAsync**(string **tableName**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| tableName | string | 소유한 캐릭터가 삽입한 GameData |

## 설명
소유중인 내 캐릭터 리스트를 불러옵니다.  
인자값 tableName을 입력할 경우, 해당 캐릭터가 tableName에 등록한 제일 최신 데이터 row 하나를 불러옵니다.  
테이블이 존재하지 않거나 해당 캐릭터가 테이블에 데이터를 생성하지 않은 경우에는 리턴되는 json에 해당 key값이 존재하지 않습니다.  
가장 최근에 생성한 캐릭터가 리스트 0번째에 위치합니다.  

## Example

### Task 방식
```js
var reqResult = await BackndMultiCharacter.Character.GetCharacterListAsync();

// 테이블과 같이 불러오기
var reqResult = await BackndMultiCharacter.Character.GetCharacterListAsync("USER_PROFILE");

```

### Callback 방식
```js
BackndMultiCharacter.Character.GetCharacterList(callback =>
{
});

BackndMultiCharacter.Character.GetCharacterList("USER_PROFILE", callback =>
{
});
```

## ReturnCase

### Success cases
**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

**캐릭터가 존재하지 않을 경우**  
statusCode : 200  
message : Success  
returnValue : `{"characters" : [ ] }`

## ReturnValueJson
```js
{
    "characters": [
        {
            "uuid": "ddf81620-13d1-11ee-9d9c-cf38f7e044c5", // 회원번호
            "nickname": "캐릭터1번", // 닉네임
            "inDate": "2023-06-26T05:45:58.818Z" // 유저 inDate
        },
        {
            "uuid": "de07f4a0-13d1-11ee-9d9c-cf38f7e044c5", // 회원번호
            "nickname": "캐릭터2번", // 닉네임
            "inDate": "2023-06-26T03:30:58.922Z", // inDate
            "USER_PROFILE": { // 함께 불러온 테이름 이름(tableName)
                "client_date": "2023-06-26T04:10:50.000Z", // Insert 시 자동 생성되는 데이터 생성일
                "dicItem": { // 데이터 내 dictionary 형태 데이터
                    "str3": "스트링3",
                    "str4": "스트링4",
                    "str1": "스트링1",
                    "str2": "스트링2"
                },
                "listItem": [ // 데이터 내 list 형태 데이터
                    1,
                    2,
                    3,
                    4,
                    5,
                    6,
                    7,
                    8,
                    9
                ],
                "strItem": "스트링", // 데이터 내 string 데이터
                "int": 10 // 데이터 내 int 데이터
            }
        },
        {
            "uuid": "ddea0c60-13d1-11ee-9d9c-cf38f7e044c5",
            "nickname": "캐릭터3번",
            "inDate": "2023-06-25T10:30:58.725Z"
        }
    ]
}
```
