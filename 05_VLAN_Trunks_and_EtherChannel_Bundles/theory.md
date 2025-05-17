# Chapter 5. VLAN Trunks and EtherChannel Bundles

## 1. VLAN Trunk란?
- 여러 VLAN의 트래픽을 하나의 링크(Trunk)로 전달
- 802.1Q 태깅 사용

## 2. Dynamic Trunking Protocol (DTP)
- 스위치 간 자동으로 트렁크 설정 협상

## 3. EtherChannel이란?
- 여러 물리적 링크를 하나의 논리적 링크로 묶어 대역폭 증가, 이중화 제공
- LACP(IEEE 802.3ad), PAgP(Cisco Proprietary) 방식

## 4. 주요 명령어
- `show interfaces trunk`
- `show etherchannel summary`
- `interface range ...`
- `channel-group <번호> mode active/on`
