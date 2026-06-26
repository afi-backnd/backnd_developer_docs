---
sidebar_label: 문자열 비속어 포함 여부 확인
draft: true
unlisted: true
---

# IsFilteredString

:::warning 채팅(신버전) 출시로 뒤끝챗 지원이 종료되었습니다.  
뒤끝챗은 모든 업데이트와 지원이 종료되었습니다.  
기존 뒤끝챗을 활성화한 프로젝트에 한하여 25년 2월 28일까지만 이용 가능합니다.  

25년 3월 1일부터 뒤끝챗의 서비스가 종료되어 기존 뒤끝챗을 이용하던 프로젝트의 경우도 더 이상 이용이 불가합니다.  
새롭게 출시된 <a href="https://docs.thebackend.io/sdk-docs/chat/intro">**채팅**</a>을 이용해 주세요.
:::

public bool **IsFilteredString**(string **message**);

## 파라미터

| Value   | Type   | Description                      |
| :------ | :----- | :------------------------------- |
| message | string | 비속어 포함 여부를 확인할 문자열 |

## 설명

해당 문자열에 비속어에 해당되는 문자열이 포함되어 있는지 확인합니다.  
리터값이 true일 경우에는 비속어가 포함된 문자열이며, false일 경우에는 비속어가 포함되지 않는 문자열입니다.  
**해당 기능을 사용하고자 할 경우에는 뒤끝 콘솔에서 뒤끝챗을 활성화하고 SetFilterUse(true)를 통해 필터링 여부를 활성화해야합니다.**  

## Example

```js
using UnityEngine;
using BackEnd;
public class NewBehaviourScript : MonoBehaviour {
    private bool isFilterUse = false;
    // Start is called before the first frame update
    void Start() {
        var bro = Backend.Chat.SetFilterUse(true);

        isFilterUse = Backend.Chat.SetFilterUse(true);

    }

    void CheckFiltering() {

        if(isFilterUse) {
            string message = "야이새끼야";
            bool isFilterString = Backend.Chat.IsFilteredString(message);

            if(isFilterString) {
                Debug.Log("욕설입니다.");
            } else {
                Debug.Log("욕설이 아닙니다");
            }
        }
    }
}

```
