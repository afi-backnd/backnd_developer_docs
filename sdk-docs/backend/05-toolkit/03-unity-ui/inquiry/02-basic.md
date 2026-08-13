---
sidebar_label: "함수 설명"
description: "함수 설명"
---

# 함수 설명

## 기능 예제코드

```js
using BackEnd;
using UnityEngine;

public class NewBehaviourScript : MonoBehaviour {

    void Start() {
        Debug.Log(Backend.Initialize(false));
        Debug.Log(Backend.BMember.CustomLogin("a0", "a0"));

        // BackendPlus.Question.SetFirstKeyActive(false); // 페이징 기능 활성화 여부
        BackendPlus.Question.SetQuestionListReloadDelaySeconds(600); // 문의 내역 재호출 딜레이 600초로 설정
        BackendPlus.Question.SetLanguageJsonName("English"); // UI 내 언어 영어로 변경(미호출 시 한글)
        BackendPlus.Question.OpenUI();
    }
}
```

## 문의창 열기

**BackendPlus.Question.OpenUI();**  

1대1문의 UI를 활성화합니다. 별도의 초기화 함수는 필요하지 않습니다.  
문의창을 열때마다 UI의 위치는 초기화됩니다.  

```js
BackendPlus.Question.OpenUI();
```

## 문의내역 불러오기 재호출 시간 설정

**BackendPlus.Question.SetQuestionListReloadDelaySeconds(int delaySeconds);**  

| 변수         | 설명                                           |
| :----------- | :--------------------------------------------- |
| delaySeconds | 문의 내역 불러오기를 재호출하기까지의 시간(초) |

문의 내역을 다시 서버에서 불러오는 대기 시간을 설정합니다.  
기본 설정은 300초입니다.  

```js
BackendPlus.Question.SetQuestionListReloadDelaySeconds(600);
```

## 페이징 기능 활성화 여부

**BackendPlus.Question.SetFirstKeyActive(bool firstKeyActive);**  

| 변수           | 설명                                                            |
| :------------- | :-------------------------------------------------------------- |
| firstKeyActive | true : 10개씩 페이징 처리 / false : 무조건 최신 10개만 불러오기 |

문의내역 UI에서 스크롤이 최하단에 있을 때 다음 문의 유형 10개를 불러오는 페이징 여부를 설정합니다.  
기본 설정은 활성화 상태입니다.  

```js
BackendPlus.Question.SetFirstKeyActive(false);
```

## 언어 변경

**BackendPlus.Question.SetLanguageJsonName(string jsonName);**  

| 변수     | 설명                         |
| :------- | :--------------------------- |
| jsonName | json 파일 이름(.json은 제외) |

`Assets > BackendPlus > UI > Question > Resources > BackendUI > Question` 폴더에 위치한 json 파일을 읽어옵니다.  
기본적으로 제공하는 파일은 Korean과 English입니다.  
다음 문의창을 활성화할 때 이전 문의창은 제거되고 새롭게 적용된 문의창으로 활성화됩니다.  

```js
BackendPlus.Question.SetLanguageJsonName("English");
```

## 문의창 닫기

**BackendPlus.Question.CloseUI();**  

1대1문의 UI를 비활성화합니다. Hierachy상에는 비활성화인 상태로 남아있게 됩니다.  
1대1문의 UI 우측 상단에 'X' 버튼 또한 해당 기능을 사용합니다.  

```js
BackendPlus.Question.CloseUI();
```

## Scene 내 UI 오브젝트 해제

**BackendPlus.Question.DestroyUI();**  

1:1 문의 UI를 hierachy와 메모리에서 해제합니다.  

```js
BackendPlus.Question.DestroyUI();
```

## 문의창 모두 삭제하기

**BackendPlus.Question.DestroyAll();**  

1:1 문의 UI를 hierachy와 메모리에서 해제하고, 할당된 데이터와 에셋도 메모리 해제합니다.  

```js
BackendPlus.Question.DestroyAll();
```
