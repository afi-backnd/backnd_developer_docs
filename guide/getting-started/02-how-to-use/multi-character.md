---
sidebar_label: 멀티 캐릭터
description: "멀티 캐릭터"
sidebar_position: 12.2
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 멀티 캐릭터

멀티 캐릭터는 **하나의 계정으로 여러 캐릭터를 만들어 플레이**할 수 있는 기능입니다.  
계정은 로그인을 담당하고, 캐릭터는 실제 게임 플레이를 담당합니다.

<ConsoleLinkButton text="유저 바로가기" menu="baseGamer/5.11.0/gamer" feature="유저" title="멀티 캐릭터" />

관련 콘솔 가이드 : [유저 관리 - 멀티 캐릭터](/guide/console-guide/backnd-base/users/multi-character)  
관련 SDK 개발자 문서 : [멀티 캐릭터란](/sdk-docs/backend/base/multi-character/what-is-multi-character)

![싱글 캐릭터 환경과 멀티 캐릭터 환경 비교](/img/docs/guide/base/multi-character/single-vs-multi-character.png)

## 이런 게임에 적합합니다

- 한 유저가 여러 캐릭터를 키우는 게임(예: MMORPG, RPG 등)
- 직업이나 진영에 따라 진행도를 따로 관리하는 게임
- 본캐와 부캐를 오가며 플레이하는 게임

## 이렇게 동작합니다

- 계정으로 로그인한 뒤, 플레이할 캐릭터를 선택해 게임을 시작합니다.
- 레벨, 인벤토리, 길드, 친구, 랭킹 등 **플레이 데이터는 캐릭터마다 따로** 관리됩니다.
- 로그인, 차단, 탈퇴, 푸시처럼 사람 단위로 판단해야 하는 처리는 **계정 기준**으로 동작합니다.

## 이런 것들을 할 수 있습니다

- 게임에서 캐릭터를 **생성 · 조회 · 선택 · 삭제**하여 캐릭터 선택 화면을 구성할 수 있습니다.
- 콘솔에서 캐릭터와 계정을 각각 조회하고, 계정에 속한 캐릭터를 한눈에 확인할 수 있습니다.
- 대시보드와 그룹 지표를 계정 기준으로 집계해 **실제 이용자 수**를 파악할 수 있습니다.
- 여러 디바이스에서 같은 계정으로 서로 다른 캐릭터를 동시에 플레이할 수 있습니다.

## 운영 중인 게임에도 적용할 수 있습니다

기존 유저를 계정으로 전환할 수 있으며, 그동안 쌓인 플레이 데이터는 계정의 하위 캐릭터로 유지됩니다.

:::caution
- 멀티 캐릭터 프로젝트로 활성화 후 SDK를 통해 각 유저를 계정으로 승격할 수 있습니다.
- 멀티 캐릭터를 활성화한 프로젝트는 이전으로 되돌릴 수 없습니다.
- 계정으로 승격한 유저는 이전으로 되돌릴 수 없습니다.
- 멀티캐릭터 관련 호출은 유저 관리 항목으로 집계되어 청구됩니다. ([베이스 요금](https://backnd.com/ko/pricing/base/))
:::
