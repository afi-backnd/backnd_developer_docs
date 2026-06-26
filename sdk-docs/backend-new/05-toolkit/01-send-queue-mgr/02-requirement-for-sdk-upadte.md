---
sidebar_label: "[Deprecated] SDK 업데이트시 주의사항"
---

# [Deprecated] SDK 업데이트 시 주의사항

:::warning 신규 버전(6.0.1이상) SDK에서는 SendQueue를 지원하지 않습니다.
신규 버전(6.0.1이상) SDK에서는 더 이상 콜백을 위한 SendQueue를 지원하지 않습니다.  
async/await 로 동작하는 비동기 메서드가 추가되어 더 쉽게 SendQueue와 동일한 동작 구현이 가능합니다.  
async/await 비동기 메서드를 사용해 주시기 바랍니다.
:::

개발사에서 SendQueueMgr을 이미 사용하고 있고, Exception 처리 등을 임의의 로직을 작성하여 사용하던 중,
SDK 업데이트 시 SDK에 기본적으로 존재하는 SendQueueMgr로 스크립트가 초기화될 수 있습니다.  

이미 SendQueueMgr을 사용하는 개발사에서는 SDK 업데이트 시 ToolKit > SendQueueMgr을 **체크해제** 한 후 SDK 업데이트를 적용할 것을 추천합니다.  

> 체크 혹은 체크 해제가 표시되지 않으면 그대로 진행하셔도 무방합니다.  

![image](https://developer.thebackend.io/static/img/ToolKit/sendQueue/update.png)
