---
sidebar_position: "3"
description: "스키마 관리"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 스키마 관리

## 1. 로그 유형 관리

로그 유형을 생성하고 관리할 수 있습니다.  
하나의 프로젝트당 최대 **500개까지 로그 유형을 생성**할 수 있습니다.

![로그스키마1](/img/docs/guide/base/logs/logs_schema1.png)


## 2. 로그 유형 별 상세 관리

로그 유형별로 `로그 보관 기간`, `로그 스키마`를 설정할 수 있습니다.  
로그 저장 시, 스키마에 정의된 필드명과 일치하는 `param key`는 해당 필드로 저장됩니다.

![로그스키마2](/img/docs/guide/base/logs/logs_schema2.png)

로그 보관 기간을 변경하면, **변경된 기간을 초과한 로그는 즉시 삭제**되며, 삭제된 로그는 **복구할 수 없습니다.**  
- 예: 60일에서 30일로 변경 시, 기존 로그 중 30일 초과된 로그는 삭제됩니다.


## 3. 미정의 로그 저장 시 정책

정의되지 않은 로그 유형은 저장 시 자동 생성되며, 이후 콘솔에서 관리할 수 있습니다.  
스키마에 정의되지 않은 param key는 모두 `unknown_fields` 필드에 JSON 형태로 저장됩니다.  
해당 param key를 **스키마에 필드로 등록하면**, **등록 이후부터 저장되는 로그**는 지정한 필드에 저장됩니다.

![로그스키마3](/img/docs/guide/base/logs/logs_schema3.png)
