---
sidebar_label: "SDK 업데이트시 주의사항"
description: "SDK 업데이트 시 주의사항"
---

# SDK 업데이트 시 주의사항

개발사에서 SendQueueMgr을 이미 사용하고 있고, Exception 처리 등을 임의의 로직을 작성하여 사용하던 중,
SDK 업데이트 시 SDK에 기본적으로 존재하는 SendQueueMgr로 스크립트가 초기화될 수 있습니다.  

이미 SendQueueMgr을 사용하는 개발사에서는 SDK 업데이트 시 ToolKit > SendQueueMgr을 **체크해제** 한 후 SDK 업데이트를 적용할 것을 추천합니다.  

> 체크 혹은 체크 해제가 표시되지 않으면 그대로 진행하셔도 무방합니다.  

![image](https://developer.thebackend.io/static/img/ToolKit/sendQueue/update.png)
