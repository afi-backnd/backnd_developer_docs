---
sidebar_label: "문의 보내기"
sidebar_position: "3"
description: "SendQuestion"
---

# SendQuestion

public BackendReturnObject **SendQuestion**(QuestionType **questionType**, string **title**, string **content**);

## 파라미터

| Value        | Type         | Description |
| :----------- | :----------- | :---------- |
| questionType | QuestionType | 문의 유형   |
| title        | string       | 문의 제목   |
| content      | string       | 문의 내용   |

### QuestionType

| Type                        | Description |
| :-------------------------- | :---------- |
| QuestionType.Account        | 계정        |
| QuestionType.Payment        | 결제        |
| QuestionType.Restore        | 복구        |
| QuestionType.GameQuestion   | 게임문의    |
| QuestionType.Report         | 신고        |
| QuestionType.SetupOrExecute | 설치/진행   |
| QuestionType.Bug            | 버그        |
| QuestionType.Event          | 이벤트      |
| QuestionType.Suggestions    | 건의사항    |
| QuestionType.Etc            | 기타        |

## 설명

문의를 발송합니다. 문의 유형은 enum QuestionType으로 지정할 수 있습니다.  

> 사전 업로드 기능은 현재 제공하고 있지 않습니다.  

## Example

### 동기

```js
Backend.Question.SendQuestion(QuestionType.Account, "제목", "내용");
```

### 비동기

```js
Backend.Question.SendQuestion(QuestionType.Account, "제목", "내용", (callback) => {

});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Question.SendQuestion, QuestionType.Account, "제목", "내용", (callback) => {

});
```

## Return Cases

### Success Cases

statusCode : 204  
message : Success  
