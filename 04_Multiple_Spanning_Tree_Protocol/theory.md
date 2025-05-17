# Chapter 4. Multiple Spanning Tree Protocol (MSTP)

## 1. MSTP란?
- 여러 VLAN을 그룹(MST 인스턴스)으로 묶어 각각 별도의 STP 인스턴스 운영
- 자원 효율적, 트래픽 분산 가능

## 2. 주요 개념
- **MST 인스턴스**: 여러 VLAN을 하나의 STP 인스턴스로 관리
- **MST Region**: 동일한 MST 설정을 가진 스위치 그룹
- **CIST (Common and Internal Spanning Tree)**: 전체 네트워크의 기본 트리

## 3. MSTP 설정 요소
- 인스턴스 번호, VLAN 매핑, 이름, revision 번호

## 4. 주요 명령어
- `show spanning-tree mst`
- `spanning-tree mode mst`
- `spanning-tree mst configuration`
