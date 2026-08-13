---
sidebar_label: "내 특정 문의 불러오기"
sidebar_position: "5"
description: "GetQuestionOne"
---

# GetQuestionOne

public BackendReturnObject **GetQuestionOne**(string **inDate**);

## 설명

자신이 보낸 문의 목록 특정 한개를 불러옵니다.  
답변이 완료된 경우에는 answerDate, answer 컬럼이 추가되며, flag가 **답변완료**로 변경됩니다.  

## Example

### 동기

```js
var bro = Backend.Question.GetQuestionOne("2023-04-07T02:03:34.707Z");
```

### 비동기

```js
Backend.Question.GetQuestionOne("2023-04-07T02:03:34.707Z", (callback) => {
	
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Question.GetQuestionOne, "2023-04-07T02:03:34.707Z", (callback) => {
	
});
```

## ReturnCase

### Success cases

**불러오기에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

**등록된 문의가 존재하지 않을 경우**  
statusCode : 200  
message : Success  
returnValue : {"rows":[]}

## GetReturnValuetoJSON

```js
{
    "row":
        {
            "content": "경쟁 요소가 있어야 더 재미있을 거 같습니다.",
            "answer": "답변드립니다.",
            "flag": "답변완료",
            "nickname": "a0",
            "inDate": "2023-04-10T07:28:09.591Z",
            "images": [],
            "type": "건의사항",
            "answerDate": "2023-04-07T05:46:01.212Z",
            "answer": "답변드립니다.",
            "title": "랭킹 기능좀 추가해주세요"
        }
}
```

## Example Code

```js
class QuestionData {
	public DateTime answerDate;
	public string content;
	public string flag;
	public string nickname;
	public string inDate;
	public string answer;
	public string title;
	public string type;
	public override string ToString() {
		return $"answerDate : {answerDate}\n" +
		$"content : {content}\n" +
		$"flag : {flag}\n" +
		$"nickname : {nickname}\n" +
		$"inDate : {inDate}\n" +
		$"answer : {answer}\n" +
		$"title : {title}\n" +
		$"type : {type}\n"
		;
	}
}
```

```js
string inDate = Backend.Question.GetQuestionList().Rows()[0]["inDate"].ToString();

LitJson.JsonData jsonData = Backend.Question.GetQuestionOne(inDate).GetFlattenJSON()["row"];

QuestionData questionData = new QuestionData();

if(jsonData.ContainsKey("answerDate")) {
	questionData.answerDate = DateTime.Parse(jsonData["answerDate"].ToString());
}

questionData.content = jsonData["content"].ToString();
questionData.flag = jsonData["flag"].ToString();
questionData.nickname = jsonData["nickname"].ToString();
questionData.inDate = jsonData["inDate"].ToString();
questionData.title = jsonData["title"].ToString();
questionData.type = jsonData["type"].ToString();

if(jsonData.ContainsKey("answer")) {
	questionData.answer = jsonData["answer"].ToString();
}

Debug.Log(questionData.ToString());
```
