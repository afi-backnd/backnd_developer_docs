---
sidebar_label: "[Todo] 멀티 프로젝트 등록"
description: "[Todo] 멀티 프로젝트 등록"
sidebar_position: 6.4
---

# 멀티 프로젝트 등록

:::danger SDK 임포트 시, 설정값 초기화
새로운 버전의 SDK를 import하였을 때, 멀티 프로젝트 기능에 새로운 기능이 추가될 경우 설정된 Multi Project 기능이 초기화 될 수 있습니다.  

import 이후 설정한 The Backend Multi Setting의 정보가 유지되었는지 확인해주시기 바랍니다.  
:::

## 멀티 프로젝트 사용 예시
멀티 프로젝트의 경우 다음과 같은 상황에서 사용할 수 있습니다.  
* 한 게임(빌드)에서 테스트 서버와 라이브 서버를 나누고자 할 경우
* 한 게임(빌드)에서 국가별로 연동되지 않는 다른 서버를 나누고자 할 경우

## 사용법

### 1. 뒤끝 인스펙터 설정
유니티 상단에 The Backend > Edit Multi Settings를 클릭합니다.  

<img src="https://developer.thebackend.io/static/img/unity/multiProject/1.png"/>

프로젝트를 추가합니다.  

<img src="https://developer.thebackend.io/static/img/unity/multiProject/2.png"/>


### 2. 코드 작성
인스펙터에서 설정한 프로젝트들은 BackEnd.MultiSettings.MultiSettingManager을 통해 불러올 수 있습니다.  
MultiSettingManager.FindByProjectName을 통해 프로젝트명을 검색하여 프로젝트를 불러올 수 있습니다.  
만약 존재하지 않는 프로젝트라면 null이 리턴됩니다.  

```js
using System.Collections.Generic;
using UnityEngine;
using BackEnd;

public class BackendManager : MonoBehaviour {
        void Start() {
        BackEnd.MultiSettings.MultiProject multiProject = BackEnd.MultiSettings.MultiSettingManager.FindByProjectName("USA");

        if(multiProject == null) {
            Debug.LogError("해당 프로젝트를 찾을 수 없습니다.");
        }
        
        var bro = Backend.InitializeByMuiltiProject(multiProject,true, true);

        if(bro.IsSuccess()) {
            Debug.Log("초기화가 성공하였습니다.");
        } else {
            Debug.Log("초기화중 에러가 발생하였습니다" + bro);
        }
    }
}
```

### 초기화 프로젝트 선택
혹은 유저들에게 프로젝트를 선택하게 할 수도 있습니다.  
다음은 저장된 프로젝트 리스트를 불러와 버튼 형태로 만들어, 클릭 시 해당 프로젝트로 초기화하는 함수입니다.  

<img src="https://developer.thebackend.io/static/img/unity/multiProject/3.png"/>

```js
    [SerializeField] private GameObject multiCharacterButtonObject;

    public void Initialize() {
        List<MultiProject> multiProjectList = MultiSettingManager.theBackendMultiSettings.projectList;
        
        for(int i = 0; i < multiProjectList.Count; i++) {
            var obj = Instantiate(multiCharacterButtonObject, multiCharacterListGroup, true);
            obj.transform.localScale = new Vector3(1, 1, 1);
            int index = i;
            obj.GetComponentInChildren<TMP_Text>().text = multiProjectList[i].projectName;
            obj.GetComponent<Button>().onClick.AddListener(() => {
                string clientAppId = MultiSettingManager.theBackendMultiSettings.projectList[index].clientAppId;
                string signatureKey = MultiSettingManager.theBackendMultiSettings.projectList[index].signatureKey;

                InitializeBackend(clientAppId, signatureKey);
            });
        }
    }

    private void InitializeBackend(string clientAppId, string signatureKey) {

        BackendCustomSetting backendCustomSetting = new BackendCustomSetting();
        
        backendCustomSetting.clientAppID = clientAppIdInputField.text;
        backendCustomSetting.signatureKey = signatureKeyInputField.text;
        backendCustomSetting.useAsyncPoll = true;
        
        var bro = Backend.Initialize(backendCustomSetting);
        
        if(bro.IsSuccess()) {
            Debug.Log("초기화가 성공했습니다!");
        } else {
             Debug.LogError("초기화 중 에러가 발생했습니다!\n" + bro.ToString());
        }
    }
```
