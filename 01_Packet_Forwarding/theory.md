
# CHAPTER 1: Packet Forwarding

## Chapter Summary
이 장에서는 네트워크 장비(스위치와 라우터)가 어떻게 L2/L3 트래픽을 전달하는지를 설명하며, 포워딩 아키텍처(Centralized vs Distributed), CEF, VLAN, Access/Trunk 포트 설정 등을 다룹니다.

---

## "Do I Know This Already?" Quiz & Answers

| 번호 | 문제 요약 | 정답 | 해설 요약 |
|------|----------|------|------------|
| 1 | L2 트래픽 전달 기준 | d | 목적지 MAC 주소 사용 |
| 2 | 충돌 도메인 분리 장비 | b | Switch는 포트별 도메인 생성 |
| 3 | L3 트래픽 전달 기준 | a, b | IP 주소 기반 전달 |
| 4 | 브로드캐스트 도메인 분리 장비 | d | Router는 L3 경계 역할 |
| 5 | MAC 주소 테이블 저장 방식 | b | CAM(Content Addressable Memory) |
| 6 | 고성능 포워딩 아키텍처 | d | Distributed 구조는 라인 카드 기반 |
| 7 | CEF 구성 요소 | b, d | FIB, Adjacency Table 포함 |

---

## Foundation Topics

### Network Device Communication
- 현대 네트워크는 TCP/IP 기반
- OSI 모델 기반 (Application ~ Physical)
- 데이터 흐름은 상위 → 하위 계층 (전송 시), 하위 → 상위 계층 (수신 시)

### Layer 2 Forwarding
- MAC 주소 기반 전달
- MAC 주소: 48비트 / OUI + 장치 ID
- 브로드캐스트 주소: `FF:FF:FF:FF:FF:FF`
- **Collision Domain**: 허브는 동일 도메인, 스위치는 포트 단위 분리

### VLAN (Virtual LAN)
- 브로드캐스트 도메인 분리
- VLAN 간 통신은 라우터 또는 Layer 3 스위치 필요
- 802.1Q Tag 구성: TPID(0x8100), PCP, DEI, VLAN ID

#### Example 1-1: VLAN 생성
```shell
SW1(config)# vlan 10
SW1(config-vlan)# name PCs
```

#### Example 1-2: VLAN 포트 매핑 조회
```shell
SW1# show vlan
```

### Access Port
- 단일 VLAN에 연결
- 태깅 없음

#### Example 1-4: Access 포트 설정
```shell
SW1(config)# interface gi1/0/15
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 99
```

### Trunk Port
- 다수의 VLAN을 전달
- 802.1Q 태깅 포함
- Native VLAN 설정 권장 (1 제외)

#### Example 1-6: Trunk 포트 상태 확인
```shell
SW1# show interfaces trunk
```

---

## Layer 2 진단 명령어

| 기능 | 명령어 |
|------|--------|
| MAC 테이블 조회 | `show mac address-table` |
| 포트 구성 확인 | `show interfaces gi1/0/x switchport` |
| 전체 인터페이스 상태 | `show interfaces status` |

---

## Layer 3 Forwarding

- ARP를 통한 MAC 주소 결정
- 목적지 IP가 다른 서브넷 → 라우터 경유

#### Example: IPv4/IPv6 주소 설정
```shell
R1(config)# interface gi0/0/0
R1(config-if)# ip address 10.10.10.254 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:10::254/64
```

### Routed Subinterfaces
```shell
R2(config)# interface g0/0/1.10
R2(config-subif)# encapsulation dot1Q 10
R2(config-subif)# ip address 10.10.10.2 255.255.255.0
```

### Switched Virtual Interface (SVI)
```shell
SW1(config)# interface vlan 10
SW1(config-if)# ip address 10.10.10.1 255.255.255.0
```

### Routed Switch Port
```shell
SW1(config)# int gi1/0/14
SW1(config-if)# no switchport
SW1(config-if)# ip address 10.20.20.1 255.255.255.0
```

---

## Forwarding Architectures

### Process Switching
- 소프트웨어(CPU) 처리 기반
- ARP 미완성 패킷 등 예외 트래픽 처리

### Cisco Express Forwarding (CEF)
- 고속 하드웨어 전환 기술
- 구성요소:
  - FIB: 목적지 IP → 다음 홉 IP
  - Adjacency Table: 다음 홉 IP → MAC 주소

### TCAM
- Value/Mask/Result 기반
- ACL, QoS, PBR 등의 빠른 매칭 처리

### Centralized vs Distributed
- Centralized: RP가 모든 처리
- Distributed: 라인카드가 개별 처리 → 확장성 우수

---

## SDM Templates

- **Switch Database Manager (SDM)**는 스위치 리소스 할당 제어
- 설정 명령:
```shell
SW1(config)# sdm prefer advanced
SW1# reload
```

#### Example 1-17: 현재 SDM 템플릿 보기
```shell
SW1# show sdm prefer
```

---

## Define Key Terms

- access port
- Address Resolution Protocol (ARP)
- broadcast domain
- Cisco Express Forwarding (CEF)
- collision domain
- content addressable memory (CAM)
- Layer 2 forwarding / Layer 3 forwarding
- Forwarding Information Base (FIB)
- MAC address table
- native VLAN
- process switching
- Routing Information Base (RIB)
- trunk port
- ternary content addressable memory (TCAM)
- virtual LAN (VLAN)

---

## Command Reference

| 작업 | 명령어 |
|------|--------|
| VLAN 정의 | `vlan vlan-id`<br>`name vlan-name` |
| Access 포트 설정 | `switchport mode access`<br>`switchport access vlan X` |
| Trunk 포트 설정 | `switchport mode trunk` |
| 정적 MAC 등록 | `mac address-table static ...` |
| ARP 테이블 보기 | `show ip arp` |
| 인터페이스 상태 확인 | `show interfaces status` |
| 인터페이스 구성 확인 | `show interfaces interface-id switchport` |
| IP 확인 | `show ip interface brief` |
| IPv6 확인 | `show ipv6 interface brief` |

---

## Exam Preparation Tasks
- 본 장의 연습문제
- Chapter 30 “Final Preparation”
- Pearson Test Prep 시뮬레이션 문제 활용

---

## 참고 문헌

- *Inside Cisco IOS Software Architecture*  
  - ISBN-13: 9781587058165
- *Cisco Express Forwarding*  
  - ISBN-13: 9780133433430
