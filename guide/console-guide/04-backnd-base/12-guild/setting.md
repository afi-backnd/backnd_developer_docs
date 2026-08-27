---
sidebar_position: "2"
description: "길드 설정"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 길드 설정

## 길드 생성 조건/가입 조건 설정

조건을 설정할 경우, 조건 값 **이상의 값**을 가진 유저만이 길드를 생성/가입할 수 있는 권한을 얻을 수 있습니다.  

:::danger 주의
해당 기능을 이용하기 위해서는 뒤끝 콘솔의 게임 정보 관리에 숫자형 데이터를 가진 table이 하나라도 존재해야 합니다.  
:::

<ConsoleLinkButton text="길드 설정 바로가기" menu="baseGuild/setting" feature="길드" title="길드 설정" />


![길드 설정](/img/docs/guide/guild/guild-04.png)

- 생성 조건은 최대 3개까지 설정할 수 있습니다.  
- 생성 조건/가입 조건에는 양의 정수만 입력할 수 있습니다.  

## 길드 마스터 교체 허용 기간

길드 마스터가 오랫동안 접속하지 않아 길드 운영이 중단되는 상황을 막기 위해, 길드원이 길드 마스터 권한을 직접 가져올 수 있도록 허용하는 설정입니다.

**길드 마스터 교체 허용 기간**을 설정하면, 길드 마스터가 해당 일수를 초과해 접속하지 않은 길드에서 길드원이 <a href="/sdk-docs/backend/base/guild/guild-master/claim-master" target="_blank">ClaimGuildMaster</a>를 호출해 길드 마스터가 될 수 있습니다.

- 허용 기간은 1일부터 365일까지 설정할 수 있습니다.
- 허용 기간을 설정하지 않으면 길드 마스터 교체 기능이 동작하지 않습니다. 이 경우 길드 조회 결과에 `masterLastLogin`, `inactivedMaster` 값이 포함되지 않으며, `ClaimGuildMaster` 호출은 statusCode 412로 실패합니다.
- 길드 마스터의 접속 기록이 없는 경우(구버전 계정)에는 비활성 상태로 판단합니다.
- 길드 마스터가 직접 위임 대상을 지정하는 길드 마스터 위임 기능과는 별개로 동작하며, 두 기능을 함께 사용할 수 있습니다.
- 이미 설정한 허용 기간을 비우면 길드 마스터 교체 기능이 다시 비활성화됩니다.
