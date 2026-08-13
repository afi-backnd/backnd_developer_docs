---
sidebar_label: "길드 가입 요청 유저 리스트 조회"
description: "GetApplicantsV3"
---

# GetApplicantsV3

public BackendReturnObject **GetApplicantsV3**();  
public BackendReturnObject **GetApplicantsV3**(int **limit**);  
public BackendReturnObject **GetApplicantsV3**(int **limit**, int **offset**);

## 파라미터

| Value  | Type | Description                                               | default |
| ------ | ---- | --------------------------------------------------------- | :-----: |
| limit  | int  | (Optional) 가입 요청 유저 리스트의 갯수                   |   100   |
| offset | int  | (Optional) 가입 요청 유저 리스트를 조회 할 시작점(offset) |    0    |

## 설명

길드에 가입 요청한 유저 리스트를 조회합니다.  
신청자 리스트는 먼저 신청한 사람이 먼저 위치하게 됩니다.  
* limit이 0 이하일 경우 100으로 호출됩니다.

> 유저1, 유저2, 유저3이 순서대로 요청할 경우, 리스트에서는 0번째로 유저1, 1번째로 유저2, 2번재로 유저3이 조회됩니다.  

## Example

### 동기

```js
Backend.Guild.GetApplicantsV3();
Backend.Guild.GetApplicantsV3(5);
Backend.Guild.GetApplicantsV3(5, 5);
```

### 비동기

```js
Backend.Guild.GetApplicantsV3((callback) => {
  // 이후 처리
});
Backend.Guild.GetApplicantsV3(5, (callback) => {
  // 이후 처리
});
Backend.Guild.GetApplicantsV3(5, 5, (callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Guild.GetApplicantsV3, (callback) => {
  // 이후 처리
});
SendQueue.Enqueue(Backend.Guild.GetApplicantsV3, 5, (callback) => {
  // 이후 처리
});
SendQueue.Enqueue(Backend.Guild.GetApplicantsV3, 5, 5, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**길드 마스터 혹은 운영진이 아닐 경우**  
statusCode : 403  
errorCode : ForbiddenException  
message : Forbidden selectApplicant, 금지된 selectApplicant

**길드에 가입하지 않은 유저가 함수를 호출한 경우**  
statusCode : 412  
errorCode : PreconditionFailed  
message : notGuildMember 사전 조건을 만족하지 않습니다.  

## GetReturnValuetoJSON

```js
{
  rows: [
    {
      inDate: { S: "2018-07-31T01:34:41.461Z" }, // 가입 요청한 유저의 indate
      nickname: { S: "customid10" }, // 가입 요청한 유저의 닉네임
    },
    {
      inDate: { S: "2018-07-31T01:49:47.260Z" },
      nickname: { S: "customid4" },
    },
  ];
}
```

## Sample Code

```js
public class ApplicantsItem
{
    public string inDate;
    public string nickname;
    public override string ToString()
    {
        return $"nickname : {nickname}\ninDate : {inDate}\n";
    }
}
```

```js
public void GetApplicantsV3()
{
    var bro = Backend.Guild.GetApplicantsV3();

    if(!bro.IsSuccess())
        return;

    List<ApplicantsItem> applicantsList = new List<ApplicantsItem>();

    LitJson.JsonData applicantsListJson = bro.FlattenRows();

    for(int i = 0; i < applicantsListJson.Count; i++)
    {
        ApplicantsItem applicant = new ApplicantsItem();

        if(applicantsListJson[i].ContainsKey("nickname"))
        {
            applicant.nickname = applicantsListJson[i]["nickname"].ToString();
        }
        applicant.inDate = applicantsListJson[i]["inDate"].ToString();

        applicantsList.Add(applicant);
        Debug.Log(applicant.ToString());
    }
}
```
