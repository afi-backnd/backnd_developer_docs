---
sidebar_label: "사용 예제"
description: "NPC 대사를 서버에서 불러오기"
---

# NPC 대사를 서버에서 불러오기

수정이 잦은 NPC의 대사를 서버에서 동적으로 불러와 효과적으로 게임을 관리하세요.  
대사 데이터를 서버에 업로드 하는 방법과 클라이언트에서 불러오는 방법을 자세히 다룹니다.

## 1. 시작하기 전에

### 유저가 존재해야 합니다.

'유저' 상단 메뉴에서 [게임 유저 생성]을 클릭해서 간단하게 유저를 생성할 수 있습니다.  
아이디 'user1', 비밀번호 '1234'로 유저를 생성하세요. 

![create user](/img/docs/guide/base/how-to/create-user.png)

## 2. 데이터 테이블 업로드하기

### NPC의 대사를 작성하세요.

[예시 파일](/files/NPC_dialogue.csv)을 다운로드 받을 수 있습니다.  
이 CSV 파일은 각 행이 NPC의 대사를 나타내며, 각 열은 다음과 같은 의미를 갖고 있습니다.

- `dialogueId`: `npcId`와 대사 번호로 구성된 고유 식별자.
- `dialogueContents`: NPC의 대사 내용.
- `npcId`: NPC의 고유 번호.
- `npcName`: 게임에서 보여주는 NPC의 이름.
- `npcEmotion`: NPC의 대사와 관련된 감정.

![sample chart](/img/docs/guide/base/how-to/sample-chart.png)

### NPC의 대사 테이블을 서버에 업로드하세요.

뒤끝 콘솔의 ‘차트’ 메뉴 상단에서 [차트 생성]을 클릭하세요.  
차트명을 “NPC_dialogue”로 입력하고 [확인]을 클릭하면, 데이터 테이블 저장 공간이 생성됩니다.

![create chart](/img/docs/guide/base/how-to/create-chart.png)

방금 만든 차트를 클릭하고, 상단의 [차트 파일 업로드] 버튼을 클릭하세요.  
그리고 NPC_dialogue.csv 파일을 업로드하면, 서버에 데이터가 올라간 것을 확인할 수 있습니다.  
대사를 변경해야 할 때, 클라이언트를 새로 빌드할 필요 없이 서버에서 빠르게 수정하세요.  

![upload csv](/img/docs/guide/base/how-to/upload-csv.png)

모달을 닫은 뒤, 파일 목록 좌측의 체크박스를 선택하고, 상단의 [차트 파일 적용]을 클릭하세요.  
여러 버전의 파일을 서버에 올리고, 클라이언트에서 기본으로 사용할 버전을 콘솔에서 지정할 수 있습니다.

## 3. 서버에서 NPC의 대사 데이터 불러오기

### 스크립트를 생성하세요.

새로운 스크립트를 생성하고, 이름을 'DataTable.cs'로 설정합니다.  
해당 스크립트를 열어 내용을 다음과 같이 수정합니다.

```js
using System.Collections.Generic;
using System.Text;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BackEnd;
using BackEnd.Content;

public class DataTable {
    private static DataTable _instance = null;

    public static DataTable Instance {
        get {
            if(_instance == null) {
                _instance = new DataTable();
            }

            return _instance;
        }
    }

    public void GetDataTable() {
        // 1단계: 차트 테이블 목록 조회
        Debug.Log("차트 테이블 목록을 불러옵니다.");
        var tableCallback = Backend.CDN.Content.Table.Get();

        if(tableCallback.IsSuccess() == false) {
            Debug.LogError("차트 테이블 목록을 불러오는 중, 에러가 발생했습니다. : " + tableCallback);
            return;
        }

        // 2단계: 차트 내용 조회
        Debug.Log("차트 내용을 불러옵니다.");
        var contentCallback = Backend.CDN.Content.Get(tableCallback.GetContentTableItemList());

        if(contentCallback.IsSuccess() == false) {
            Debug.LogError("차트 내용을 불러오는 중, 에러가 발생했습니다. : " + contentCallback);
            return;
        }

        Debug.Log("데이터 불러오기에 성공했습니다.");

        // 3단계: 차트 ID로 데이터 조회
        Dictionary<string, ContentItem> dic = contentCallback.GetContentDictionarySortByChartId();

        foreach (string chartId in dic.Keys)
        {
            ContentItem item = dic[chartId];
            Debug.Log($"차트명: {item.chartName}, 차트ID: {chartId}");

            // "NPC_dialogue" 차트 찾기
            if(item.chartName == "NPC_dialogue") {
                LitJson.JsonData json = item.contentJson;;

                foreach(LitJson.JsonData row in json) {
                    StringBuilder content = new StringBuilder();
                    content.AppendLine("dialogueId: " + row["dialogueId"].ToString());
                    content.AppendLine("dialogueContents: " + row["dialogueContents"].ToString());
                    content.AppendLine("npcId: " + row["npcId"].ToString());
                    content.AppendLine("npcName: " + row["npcName"].ToString());
                    content.AppendLine("npcEmotion: " + row["npcEmotion"].ToString());

                    Debug.Log(content.ToString());
                }
            }
        }
    }
}
```

### **BackendManager.cs에서 함수를 호출하세요.**

위에서 작성한 함수가 실행되려면, 게임을 실행했을 때 자동으로 실행되는 BackendManager에서 호출해야 합니다.  
뒤끝 초기화와 뒤끝 로그인이 이루어진 이후에 함수들을 호출하세요.

```js
using UnityEngine;
using System.Threading.Tasks;

// 뒤끝 SDK namespace 추가
using BackEnd;

public class BackendManager : MonoBehaviour {
    void Start() {
        var bro = Backend.Initialize(); // 뒤끝 초기화

        // 뒤끝 초기화에 대한 응답값
        if(bro.IsSuccess()) {
            Debug.Log("초기화 성공 : " + bro); // 성공일 경우 statusCode 204 Success
        } else {
            Debug.LogError("초기화 실패 : " + bro); // 실패일 경우 statusCode 400대 에러 발생
        }

        Test();
    }

    // 동기 함수를 비동기에서 호출하게 해주는 함수(유니티 UI 접근 불가)
    async void Test() {
        await Task.Run(() => {
            BackendLogin.Instance.CustomLogin("user1", "1234"); // 뒤끝 로그인 함수

            // highlight-start
            // 데이터 테이블 불러오기 로직 추가
            // CDN 차트 기능을 사용하여 차트명으로 자동으로 데이터를 불러옵니다.
            DataTable.Instance.GetDataTable();
            // highlight-end

            Debug.Log("테스트를 종료합니다.");
        });
    }
}
```
