# Chapter 1. Packet Forwarding (패킷 포워딩)

## 1. 패킷 포워딩이란?
네트워크 장비(스위치, 라우터 등)가 수신한 패킷을 목적지로 전달하는 과정입니다.

- **L2 스위치**: MAC 주소 기반 포워딩
- **L3 라우터**: IP 주소 기반 포워딩

## 2. 주요 개념
- **Collision Domain**: 충돌이 발생할 수 있는 네트워크 구간 (허브/공유기 등)
- **Broadcast Domain**: 브로드캐스트 패킷이 도달할 수 있는 범위 (VLAN, 라우터로 분리)
- **Access Port / Trunk Port**:  
  - Access: 단일 VLAN  
  - Trunk: 여러 VLAN 태그 처리

## 3. 포워딩 방식
- **Layer 2 Forwarding**:  
  - MAC 주소 테이블(MAC Address Table) 기반  
  - 스위치가 목적지 MAC 주소를 보고 포트 결정
- **Layer 3 Forwarding**:  
  - 라우팅 테이블(Routing Table) 기반  
  - 목적지 IP 주소를 보고 다음 홉 결정

## 4. 주요 명령어 (Cisco IOS)
- `show mac address-table`
- `show ip route`
- `show interfaces`
- `show vlan brief`

---

## 5. 요약
- L2는 MAC, L3는 IP 기반으로 포워딩
- 스위치/라우터의 테이블(주소 테이블, 라우팅 테이블) 구조와 동작 원리 이해가 핵심
