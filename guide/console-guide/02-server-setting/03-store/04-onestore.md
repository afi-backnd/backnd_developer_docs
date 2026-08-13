---
description: "원스토어"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 원스토어

:::info 원스토어 정보 입력  
다음 정보는 영수증 검증 기능 이용을 위해 필요한 정보입니다.  
해당 정보를 입력 시, 뒤끝 콘솔의 영수증 검증 기능을 이용할 수 있습니다.  
해당 정보는 모두 [ONE store developer center](https://dev.onestore.co.kr/devpoc/index.omp)에서 획득할 수 있습니다.  
:::

- client ID
- Client Secret

<ConsoleLinkButton text="스토어 정보 바로가기" menu="settingStore" feature="스토어 정보" title="원스토어" />

<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/onestore_01.png" />

## client ID & Client Secret  

**client ID** 와 **Client Secret**은 **S2S API** 인증을 위해 필요한 값입니다.  

> 1. [ONE store developer center](https://dev.onestore.co.kr/devpoc/index.omp)에서 **APPS > 상품 현황 > 등록하고자 하는 상품**으로 이동합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/onestore_02.png" />  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/onestore_03.png" />  

> 2. **상품 현황 > In-App 정보 > 관리 상품 > In-App API 관리** 버튼을 눌러 관리창을 생성합니다.  
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/onestore_04.png" />  

> 3. 관리창 내 **client ID** 와 **Client Secret**을 뒤끝 콘솔에 입력합니다.
<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/onestore_05.png" />  
