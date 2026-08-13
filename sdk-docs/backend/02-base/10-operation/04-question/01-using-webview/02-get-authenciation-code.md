---
sidebar_label: "1대1 문의 인증코드 받기"
sidebar_position: "2"
description: "GetQuestionAuthorize"
---

# GetQuestionAuthorize

public BackendReturnObject **GetQuestionAuthorize**();

## 설명

1대1 문의에서 문의를 보내려면 문의 창을 열 때(OpenQuestionView를 호출할 때) 인자 값으로 인증번호를 입력해야 합니다.  
입력 코드가 올바르지 않을 경우 문의 내역을 불러오거나 문의를 등록할 때 비정상적으로 동작하게 됩니다.  

## Example

### 동기

```js
BackendReturnObject bro = Backend.Question.GetQuestionAuthorize();
string questionAuthorize = bro.GetReturnValuetoJSON()["authorize"].ToString();
```

### 비동기

```js
Backend.Question.GetQuestionAuthorize((callback) => {
   string questionAuthorize = callback.GetReturnValuetoJSON()["authorize"].ToString();
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Question.GetQuestionAuthorize, (callback) => {
   string questionAuthorize = callback.GetReturnValuetoJSON()["authorize"].ToString();
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

성공 시, OpenQuestionView에 사용될 인증코드를 나타냅니다.  

```js
{"authorize":"1ea3f14d34e89584df69c55332657a2432821cec9b701eac55332657a243283f14d34e89584df69c55332657a2ec9b70"}
```
