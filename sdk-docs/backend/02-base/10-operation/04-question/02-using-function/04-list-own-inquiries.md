---
sidebar_label: "내 문의 리스트 불러오기"
sidebar_position: "4"
description: "GetQuestionList"
---

# GetQuestionList

public BackendReturnObject **GetQuestionList**();  
public BackendReturnObject **GetQuestionList**(string **firstKeyString**);

## 설명

자신이 보낸 문의 목록을 불러옵니다.  
최대 10개까지 불러올 수 있으며, 10개 이상의 문의는 firstKey를 이용하여 조회해야합니다.  
firstKey가 string.Empty일 경우에는 제일 최신 문의로부터 10개를 불러옵니다.(`GetQuestionList()` 함수와 동일한 결과)
답변이 완료된 경우에는 answerDate, answer 컬럼이 추가되며, flag가 **답변완료**로 변경됩니다.  

## Example

### 동기

```js
var bro = Backend.Question.GetQuestionList();
```

### 비동기

```js
Backend.Question.GetQuestionList((callback) => {

});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Question.GetQuestionList, (callback) => {
	
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

**firstKey가 존재하는 경우(불러올 문의가 더 존재할 경우)**  

```js
{
    "rows": [
        {
            "content": "경쟁 요소가 있어야 더 재미있을 거 같습니다.",
            "flag": "답변대기",
            "nickname": "a0",
            "inDate": "2023-04-10T07:28:09.591Z",
            "images": [],
            "type": "건의사항",
            "title": "랭킹 기능좀 추가해주세요"
        },
        {
            "answerDate": "2023-04-11T04:46:43.783Z", // 결제가 완료된 건입니다.  
            "content": "7시 48분에 결제한 골드 1000개가 들어오지 않았습니다.",
            "flag": "답변완료",
            "nickname": "뒤끝마스터",
            "inDate": "2023-04-10T08:41:49.490Z",
            "images": [],
            "answer": "죄송합니다. 확인해본 결과 실제 지급이 되지 않아 우편을 통해 발송해드렸습니다. 감사합니다.",
            "title": "결제 아이템 미지급"
            "type": "결제"
        }
    ],
    "firstKey": {
        "gamer_id": {
            "S": "62857bd0-dbed-11ec-84d6-89b591e5ae87_OneOnOneQuestion"
        },
        "inDate": {
            "S": "2023-04-10T07:19:23.222Z"
        }
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
List<QuestionData> questionDataList = new List<QuestionData>();

// 페이징을 위한 키(10개를 불러온 이후, 해당 부분부터 10개를 더 불러올 수 있음)
string firstKeyString = string.Empty;

do {
        // firstKey가 string.Empty일 경우에는 제일 첫 문의를 불러온다(GetQuestionList() 와 동일한 결과)
	var bro = Backend.Question.GetQuestionList(firstKeyString);
        firstKeyString = bro.FirstKeystring();

	foreach(LitJson.JsonData jsonData in bro.Rows()) {
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

		questionDataList.Add(questionData);
	}
}
while(string.IsNullOrEmpty(firstKeyString) == false);

foreach(var data in questionDataList) {
	Debug.Log(data.ToString());
}
```
