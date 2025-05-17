# Chapter 2. Spanning Tree Protocol (STP)

## 1. STP란?
- 이더넷 네트워크에서 루프를 방지하기 위한 프로토콜
- IEEE 802.1D 표준
- 스위치 간에 루프가 생기면 브로드캐스트 스톰, MAC 플러딩 등 심각한 장애 발생

## 2. 주요 개념
- **Root Bridge**: 네트워크에서 중심이 되는 스위치
- **BPDU(Bridge Protocol Data Unit)**: STP 정보 교환용 프레임
- **Port Roles**: Root Port, Designated Port, Non-Designated Port
- **Port States**: Blocking, Listening, Learning, Forwarding, Disabled

## 3. STP 동작 원리
1. Root Bridge 선출 (Bridge ID가 가장 낮은 스위치)
2. 각 스위치는 Root Bridge까지의 최단 경로 계산
3. 루프가 발생할 수 있는 포트는 Blocking 상태로 전환

## 4. 주요 명령어
- `show spanning-tree`
- `show spanning-tree vlan <번호>`
- `show spanning-tree root`
