---
sidebar_label: "아이디 찾기"
---

# FindCustomId
public Task&lt;RequestResult&gt; **FindCustomIdAsync**(string **EmailAddress**);

## 파라미터

| Value        | Type           | Description  |
| :------------ |:------------| :-----|
| EmailAddress | string | 사전에 입력한 이메일 주소 |

## 설명
커스텀 유저가 아이디를 잊어버렸거나 비밀번호를 초기화하고 싶을 때, 미리 등록해둔 유저의 이메일을 통해 아이디를 찾고, 비밀번호를 초기화할 수 있습니다.  

* 이메일 등록은 UpdateCustomEmail 함수를 참고하세요.  
* 유저에게 보낼 이메일 양식은 콘솔 > 게임 운영 관리 > 계정 정보 찾기에서 설정할 수 있습니다.  
* 보낸 이메일은 no-reply@backnd.com 이며, 보낸 사람은 프로젝트명입니다.  
* 유저 정보 찾기는 이메일 계정당 24시간 이내에 최대 아이디 찾기 5회, 비밀번호 초기화 5회까지만 가능합니다.  
* 국가 정보는 제일 처음 해당 이메일에 등록된 아이디의 국가 정보로 보여집니다.(미국 유저, 일본 유저, 한국 유저 순으로 동일한 이메일을 등록할 경우 미국 국가로 작성된 아이디 찾기 창이 보여집니다.)

해당 이메일이 등록된 아이디 정보를 모두 전송합니다.  
> 이메일이 등록된 사람들 중, 가장 먼저 가입한 사람의 국가 코드를 기준으로, 해당 국가의 제목과 내용으로 메일이 전송됩니다.  

## Example

### Task 방식
```js
var reqResult = await BackndAuth.Instance.FindCustomIdAsync("help@backnd.com");
```

### Callback 방식
```js
BackndAuth.Instance.FindCustomIdAsync("help@backnd.com", (callback) =>
{
    // 이후 처리
});
```


## ReturnCase

### Success cases

**이메일 송신에 성공한 경우**  
statusCode : 204  
message : Success  

### Error cases

**프로젝트 명에 특수문자가 추가된 경우(안내 메일 미발송 및 에러 발생)**  
statusCode : 400  
errorCode : InvalidParameterValue  
message : User name is missing: 고유값 no-reply@backnd.com;

**해당 이메일의 게이머가 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : gamer not found, gamer을(를) 찾을 수 없습니다

**24시간 이내에 5회 이상 같은 이메일 정보로 아이디 찾기를 시도한 경우**  
statusCode : 429  
errorCode : Too Many Request  
message : 정보보호를 위해 아이디 찾기/비밀번호 초기화는 하루 5회로 제한됩니다. 내일 다시 시도해 주세요. 요청 횟수를 초과하였습니다.  

