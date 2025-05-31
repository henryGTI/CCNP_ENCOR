# CHAPTER 1: Packet Forwarding

## Network Device Communication

이 섹션은 스위치가 어떻게 **Layer 2 관점**에서 트래픽을 전달하고, 라우터가 **Layer 3 관점**에서 트래픽을 전달하는지를 설명합니다.

## Forwarding Architectures

이 섹션은 라우터 및 스위치에서 네트워크 트래픽을 전달하는 데 사용되는 메커니즘을 살펴봅니다.

> 이 장은 네트워크 기본 개념에 대한 복습을 제공하고, 라우터나 스위치를 통해 네트워크 트래픽이 어떻게 전달되는지와 관련된 기술 개념을 더 깊이 있게 다룹니다.

---

## "Do I Know This Already?" Quiz

**"Do I Know This Already?"** 퀴즈는 해당 장을 읽어야 할지 판단할 수 있게 해줍니다.  
이 자기평가 질문 중 1~2개 이하로 틀렸다면, “**Exam Preparation Tasks**” 섹션으로 넘어가도 됩니다.  
이 표에는 이 장의 주요 제목과 퀴즈 문항이 매핑되어 있습니다.  
Appendix A의 *"Answers to the 'Do I Know This Already?'"* 섹션에서 답안을 확인할 수 있습니다.

### Table 1-1 "Do I Know This Already?" Foundation Topics Section-to-Question Mapping

| Foundation Topics Section      | Questions |
|--------------------------------|-----------|
| Network Device Communication   | 1–4       |
| Forwarding Architectures       | 5–7       |

---

## Quiz 문제 및 해설

### 1. Layer 2 관점에서 네트워크 트래픽 전달 시 사용하는 정보는 무엇인가?

**정답**: d. Destination MAC address

**해설**:
- **Layer 2 (Data Link Layer)**에서는 **MAC 주소**(Media Access Control address)를 기반으로 프레임을 전달합니다.
- 특히 **스위치**는 **목적지 MAC 주소**를 기준으로 해당 프레임을 어느 포트로 전달할지 결정합니다.

**오답 해설**:
- a, b: IP 주소는 Layer 3 정보입니다.
- c: 출발지 MAC 주소는 스위치가 학습은 하지만, 전달 결정에는 **목적지 MAC 주소**가 사용됩니다.
- e: Data protocol은 Layer 3 이상에서 사용됩니다.

---

### 2. 충돌 도메인(collision domain)의 크기를 줄이는 데 도움이 되는 네트워크 장비는?

**정답**: b. Switch

**해설**:
- **스위치**는 포트마다 별도의 **충돌 도메인**을 생성합니다.
- 반면 **허브(Hub)**는 모든 포트가 **동일한 충돌 도메인**에 속하므로 충돌을 줄이지 못합니다.

**오답 해설**:
- a. Hub: 모든 포트가 하나의 충돌 도메인입니다.
- c. Load balancer: 트래픽 분산 기능은 있지만 충돌 도메인과는 무관합니다.
- d. Router: 브로드캐스트 도메인 분리에 도움은 되지만, 충돌 도메인과는 직접적 관련이 없습니다.

---

### 3. Layer 3 관점에서 네트워크 트래픽 전달 시 사용하는 정보는 무엇인가?

**정답**:  
a. Source IP address  
b. Destination IP address

**해설**:
- **Layer 3 (네트워크 계층)**에서는 **IP 주소** 기반으로 트래픽을 전달합니다.
- **라우터**는 목적지 IP 주소를 기준으로 **라우팅 결정을 수행**하며, 경우에 따라 **소스 IP 주소**도 참조됩니다 (예: 정책 기반 라우팅).

**오답 해설**:
- c, d: MAC 주소는 Layer 2 정보입니다.
- e: Data protocol은 IP 헤더 내의 프로토콜 필드(TCP, UDP 등)를 의미하지만, **라우팅 결정에는 IP 주소가 핵심**입니다.

### 4. 브로드캐스트 도메인의 크기를 줄이는 데 도움이 되는 네트워크 장비는?

**정답**: d. Router

**해설**:
- 라우터는 브로드캐스트 트래픽을 인터페이스 간 전달하지 않음
- VLAN도 브로드캐스트 도메인 분리 가능 (Layer 3 스위치 필요)

**오답 해설**:
- a. Hub: 단일 브로드캐스트 도메인
- b. Switch: VLAN 없이 브로드캐스트 도메인 분리 불가
- c. Load balancer: 트래픽 분산 전용

---

### 5. MAC 주소 테이블과 직접 연관될 수 있는 것은?

**정답**: b. CAM

**해설**:
- MAC 주소 테이블은 **CAM (Content Addressable Memory)**에 저장됨

**오답 해설**:
- a. adjacency table: L3 정보
- c. TCAM: 복합 조건 처리 (ACL 등)
- d. routing table: Layer 3 라우팅 정보

---

### 6. 포트 밀도 및 전달 확장성을 향상시키는 forwarding architecture는?

**정답**: d. distributed

**해설**:
- 포트/라인 카드에 포워딩 기능 분산 → 고성능 및 확장성 제공

**오답 해설**:
- a. centralized: 병목 발생 가능
- b. clustered: 아키텍처 명칭 아님
- c. software: 성능 저하 우려

---

### 7. CEF는 어떤 구성 요소들로 구성되어 있는가? (두 개 선택)

**정답**:  
b. Forwarding Information Base  
d. Adjacency table

**해설**:
- **FIB**: 목적지 IP 기준 포워딩 경로 저장 (RIB 기반)
- **Adjacency Table**: 인접 장비의 Layer 2 주소 정보 (ARP 기반)

**오답 해설**:
- a. RIB: FIB의 기반이지만 CEF 구성 요소는 아님
- c. Label Information Base: MPLS 관련
- e. MAC address table: L2 스위칭 정보

---

## Foundation Topics

### Network Device Communication

네트워크의 주요 기능은 **장치 간의 연결성**을 제공하는 것입니다. 과거에는 장치마다 선호하는 네트워크 프로토콜이 다양했으나, 오늘날 대부분의 네트워크는 **TCP/IP (Transmission Control Protocol/Internet Protocol)** 기반입니다.

TCP/IP는 **OSI (Open Systems Interconnection)** 모델을 기반으로 한 개념적 모델이며, OSI 모델은 **7개의 계층**으로 구성되어 있습니다.  
각 계층은 특정 기능을 수행하며, 한 계층은 다른 계층을 변경하지 않고도 수정이 가능합니다.

> OSI 모델은 공급업체 간의 호환성을 위한 구조화된 접근 방식을 제공합니다.

#### Figure 1-1 OSI Model

| 계층 | 이름         | 설명                        |
|------|--------------|-----------------------------|
| 7    | Application  | 데이터 송수신 인터페이스       |
| 6    | Presentation | 데이터 형식 지정 및 암호화     |
| 5    | Session      | 패킷 추적                    |
| 4    | Transport    | 종단 간 통신                  |
| 3    | Network      | 논리적 주소 지정 및 패킷 라우팅 |
| 2    | Data Link    | 하드웨어 주소 지정             |
| 1    | Physical     | 미디어 유형 및 커넥터          |

---

### 데이터 흐름 개요

대부분의 네트워크 트래픽은 **애플리케이션 간 통신**을 포함합니다.  
- 애플리케이션은 **7계층(Application Layer)**에서 데이터를 생성합니다.  
- 장치/호스트는 데이터를 OSI 모델을 따라 **하위 계층으로 캡슐화**하여 전송합니다.  
- 데이터는 필요에 따라 **계층별로 캡슐화 및 수정**됩니다.

**Layer 3 (Network Layer)**는 데이터의 목적지를 판단합니다:  
- **같은 장치 내의 애플리케이션**이면 상위 계층으로 이동  
- **다른 장치로 전송**해야 한다면 1계층까지 내려가 전송 처리

**Layer 1 (Physical Layer)**는 전기적 신호로 데이터를 전송하며,  
수신 측에서는 **1계층부터 7계층까지 역순으로 데이터를 처리**합니다.

---

## Layer 2 Forwarding

**데이터 링크 계층 (Layer 2)**은 IP 프로토콜 아래에서 **MAC 주소**를 기반으로 데이터를 전달합니다.  
- 고유한 출발지 및 목적지 주소를 사용하여 호스트 간 통신을 수행합니다.  
- 본 문서에서는 **Ethernet 기반 Layer 2 전달**과 **MAC 주소 기반 주소 지정 방식**에 중점을 둡니다.

> **Note**:  
> - MAC 주소는 **48비트(6 옥텟)**로 구성되며, **16진수**로 표기됩니다.  
> - **처음 3 옥텟**은 제조사 식별자(OUI), 나머지 **3 옥텟**은 고유 장치 식별자입니다.  
> - 장치는 **자신의 MAC 주소**일 때만 프레임을 수신 및 처리하고 상위 계층으로 전달합니다.  
> - `FF:FF:FF:FF:FF:FF`는 **브로드캐스트 MAC 주소**로, 네트워크 상의 모든 장치가 수신합니다.  
> - 브로드캐스트는 일반적으로 **Layer 3 경계를 넘지 않습니다.**

---

### 🔑 Key Topic: Collision Domains

이더넷은 **공유 통신 매체**이며, 동일 세그먼트의 두 장치가 동시에 데이터를 전송하면 **충돌(Collision)**이 발생할 수 있습니다.

이 문제를 방지하기 위해 **CSMA/CD (Carrier Sense Multiple Access/Collision Detect)** 방식이 도입되었습니다.

- **Collision Domain**은 동일한 통신 세그먼트로, 이 범위 내에서는 충돌이 발생할 수 있습니다.
- **스위치(Switch)**는 **MAC 주소 테이블**을 기반으로 트래픽을 특정 포트로만 전달하여 충돌 도메인을 줄입니다.
- 반면, **허브(Hub)**는 트래픽을 모든 포트로 **플러딩(Flooding)**합니다.

> 따라서 스위치는 충돌 도메인을 분리하여 **네트워크 효율성을 높이는 장치**입니다.

---

## Figure 1-2: Collision Domains on a Hub Versus a Switch

- **왼쪽 그림 (허브)**: 세 장치가 동일한 **충돌 도메인**에 존재
- **오른쪽 그림 (스위치)**: 각 장치가 **서로 다른 충돌 도메인**에 존재

브로드캐스트 트래픽은 LAN의 모든 장치로 전송되며, 각 스위치 포트에도 전달됩니다.  
과도한 브로드캐스트는 **네트워크 효율을 저하**시킬 수 있습니다.  
브로드캐스트 도메인은 **Layer 3 경계를 넘지 않으며**, **같은 서브넷** 내 장치에만 영향을 미칩니다.

---

## Virtual LANs (VLAN)

**VLAN**(Virtual LAN)은 동일 네트워크 스위치 내에서 논리적인 세분화를 제공하여  
- **브로드캐스트 도메인을 줄이고**,  
- **향상된 네트워크 분리**를 가능하게 합니다.

> VLAN을 활용하면 스위치 포트를 효율적으로 구성할 수 있으며,  
> 각 포트를 특정 **브로드캐스트 도메인**에 할당할 수 있습니다.

- VLAN 간 통신은 **라우터(또는 L3 스위치)** 없이는 불가능합니다.
- 서로 다른 VLAN에 속한 장치는 같은 스위치에 있더라도 **직접 통신할 수 없습니다.**

---

## IEEE 802.1Q VLAN Tagging

**IEEE 802.1Q**는 VLAN 태깅을 위한 **32비트 필드** 구조를 정의합니다.

### VLAN 헤더 구조

| 필드명                      | 비트 수 | 설명 |
|----------------------------|---------|------|
| TPID (Tag Protocol ID)     | 16      | 0x8100으로 고정 (VLAN 식별) |
| PCP (Priority Code Point)  | 3       | 트래픽 우선순위 (QoS) |
| DEI (Drop Eligible Ind.)   | 1       | 혼잡 시 드롭 가능성 |
| VLAN ID                    | 12      | VLAN 식별자 (0 ~ 4095) |

> VLAN ID는 총 **4096개(0~4095)** 가능

---

## Figure 1-4: VLAN Packet Structure

VLAN이 포함된 Ethernet 프레임 구조는 다음과 같습니다:

- Destination MAC
- Source MAC
- TPID (0x8100)
- PCP (3-bit)
- DEI (1-bit)
- VLAN ID (12-bit)
- Source IP
- Destination IP
- Payload

> **Note**:  
> - VLAN 헤더는 일반적으로 스위치 내에서 **로컬로 전달되는 패킷에는 추가되지 않습니다.**  
> - VLAN 헤더는 **Trunk 포트**를 통해 전달되는 프레임에만 포함됩니다.

---

## Catalyst 스위치에서의 VLAN ID 범위

| VLAN ID 범위         | 용도/제약 사항                        |
|----------------------|----------------------------------------|
| VLAN 0               | 802.1p 트래픽용으로 예약 (삭제/변경 불가) |
| VLAN 1               | 기본 VLAN (삭제/변경 불가)              |
| VLAN 2 ~ 1001        | 표준 범위 (추가/삭제/수정 가능)         |
| VLAN 1002 ~ 1005     | 예약됨 (삭제 불가)                     |
| VLAN 1006 ~ 4094     | 확장 VLAN 범위 (수정 가능)             |

---

## Example 1-1: Creating VLANs in Cisco IOS

```shell
SW1# configure terminal
SW1(config)# vlan 10
SW1(config-vlan)# name PCs
SW1(config)# vlan 20
SW1(config-vlan)# name Phones
SW1(config)# vlan 99
SW1(config-vlan)# name Guest
```

위 예시는 VLAN 10, 20, 99를 생성하고 각각 이름을 지정하는 명령어입니다.

## Example 1-2: Viewing VLAN Assignments to Port Mapping

```shell
SW1# show vlan
VLAN	Name	Status	Ports
1	default	active	Gi1/0/1, Gi1/0/2, Gi1/0/3, Gi1/0/4, Gi1/0/5, ...
10	PCs	active	Gi1/0/7, Gi1/0/8, Gi1/0/9
20	Phones	active	Gi1/0/12, Gi1/0/13
99	Guest	active	Gi1/0/15, Gi1/0/16
1002–1005	—	act/unsup	(미지원 VLAN)
```
show vlan 명령은 VLAN ID, 이름, 상태, 할당된 포트 목록을 출력합니다.

---

## Example 1-3: Using the Optional show vlan Keywords

VLAN 요약 정보 출력
```
SW1# show vlan brief
VLAN 이름, 상태, 포트 목록을 간략하게 요약하여 보여줍니다.
```
특정 VLAN ID로 필터링
```
SW1# show vlan id 99
VLAN ID 99에 대한 세부 정보만 출력합니다.
```
VLAN 이름으로 필터링
```
SW1# show vlan name Guest
VLAN 이름이 **"Guest"**인 항목만 선택적으로 출력합니다.
```
show vlan 명령은 VLAN 설정 현황을 점검하거나 포트와 VLAN 간 매핑을 확인할 때 유용합니다.

---

## Key Topic: Access Ports

**Access 포트**는 스위치에서 가장 기본적인 포트 유형이며, **하나의 VLAN에만 연결**됩니다.

> Access 포트는 지정된 VLAN에서 장치로, 또는 그 장치로부터 동일 VLAN 내 다른 장치로 트래픽을 전달합니다.

- Access 포트는 **802.1Q 태그를 포함하지 않으며**, 일반적으로 **단말 장치(PC, IP Phone 등)**와 연결됩니다.
- VLAN 간 통신을 위해서는 **라우팅**이 필요합니다.

---

## Example 1-4: Configuring an Access Port

```shell
SW1# configure terminal
SW1(config)# vlan 99
SW1(config-vlan)# name Guests

SW1(config)# interface gi1/0/15
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 99

SW1(config)# interface gi1/0/16
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan name Guest
VLAN 99을 생성하고 이름을 Guests로 설정합니다.

Gi1/0/15, Gi1/0/16 포트를 Access 모드로 설정하고 VLAN 99에 할당합니다.
```

참고: switchport access vlan name Guest 명령은 잘못된 형식이며, 올바른 명령은 switchport access vlan 99입니다.

구성 확인
```
SW1# show running-config | begin interface gigabitethernet1/0/15
출력 예시:
```
```
interface GigabitEthernet1/0/15
 switchport access vlan 99
 switchport mode access
interface GigabitEthernet1/0/16
 switchport access vlan 99
 switchport mode access
```

위 출력은 지정된 포트가 Access 모드이며, VLAN 99에 바인딩되어 있음을 나타냅니다.

---

## Key Topic: Trunk Ports

**Trunk 포트**는 여러 **VLAN의 트래픽을 동시에 전달**할 수 있는 포트 유형입니다.

- Trunk 포트는 VLAN 태그가 포함된 헤더를 해석하여, **올바른 VLAN**으로 트래픽을 전달합니다.
- **스위치–스위치, 스위치–라우터, 스위치–방화벽 간 연결**에서 주로 사용됩니다.
- 하이퍼바이저 기반 **가상 스위치**가 trunk 포트를 통해 여러 VLAN과 통신할 수 있습니다.

---

### 트렁크 포트 설정 예제

```shell
SW1(config)# interface gi1/0/2
SW1(config-if)# switchport mode trunk
```

---

## Example 1-6: Verifying Trunk Port Status
```
SW1# show interfaces trunk
```
출력은 3개의 섹션으로 나뉩니다:

- 트렁크 포트, 상태, Native VLAN
- 트렁크 포트를 통해 허용된 VLAN
- STP 상태에서 포워딩 가능한 VLAN

Native VLANs
Native VLAN은 802.1Q 태그가 없는 트래픽이 전달되는 VLAN입니다.
기본값은 VLAN 1입니다.

변경 명령어:
```
SW1(config-if)# switchport trunk native vlan <vlan-id>
```

Cisco는 보안상의 이유로 VLAN 1이 아닌 사용되지 않는 VLAN을 Native VLAN으로 설정할 것을 권장합니다.
주의: Native VLAN이 서로 다른 포트 간에 일치하지 않으면 통신 오류가 발생할 수 있습니다.

허용된 VLAN (Allowed VLANs)
트렁크 포트는 특정 VLAN만 허용하도록 설정하여,
불필요한 브로드캐스트 트래픽 및 MAC 학습 오버헤드를 줄일 수 있습니다.

```
SW1(config-if)# switchport trunk allowed vlan 1,10,20,99
```

명령어 전체 구문:
```
switchport trunk allowed vlan {
  vlan-ids |
  all |
  none |
  add vlan-ids |
  remove vlan-ids |
  except vlan-ids
}
```
⚠️ add와 remove를 생략하면 설정이 **덮어쓰기(overwrite)**되어 기존 VLAN이 삭제될 수 있습니다.

Layer 2 진단 명령어: MAC 주소 테이블
```
show mac address-table [address <mac-address> | dynamic | vlan <vlan-id>]
```
옵션 설명:

- address: 특정 MAC 주소 검색
- dynamic: 동적으로 학습된 MAC 항목만 표시
- vlan: 특정 VLAN에 대한 MAC 정보 조회

---

## Example 1-8: Viewing the MAC Address Table
```
SW1# show mac address-table dynamic
```
예시 출력에서 Gi1/0/3 포트에 여러 MAC 주소가 있는 경우,
→ 해당 포트에 허브 또는 가상 스위치가 연결되었을 가능성이 있습니다.

Key Topic: Switch Port Status
상세 포트 설정 조회
```
show interfaces gi1/0/5 switchport
```
포트 모드
- Native VLAN
- Voice VLAN
- 프라이빗 VLAN 상태
- Trunk 여부 등을 확인할 수 있습니다.

Interface Status 요약 보기
```
show interfaces status
```
출력 항목: 포트 ID, VLAN, Duplex, 속도, 유형 등
포트 전체 상태를 간결하게 한눈에 확인할 수 있는 유용한 명령입니다.

---

## Example 1-10: Viewing Overall Interface Status

```shell
SW1# show interfaces status
```
이 명령어는 각 포트의 전반적인 상태를 간단한 테이블 형식으로 출력합니다.

- 포트	상태	VLAN	Duplex	속도	유형
- Gi1/0/1	notconnect	1	auto	auto	10/100/1000BaseTX
- Gi1/0/2	connected	trunk	a-full	1000	10/100/1000BaseTX
- notconnect: 포트가 연결되어 있지 않음
- connected: 정상적으로 연결된 상태
- VLAN: Access 포트일 경우 VLAN 번호, Trunk일 경우 trunk로 표시
- Duplex: auto, a-full(자동 협상된 full duplex) 등
- 속도: 자동 협상 또는 고정된 속도
- 유형(Type): 물리적 인터페이스 유형 (예: 10/100/1000BaseTX)

이 명령어는 포트 상태를 빠르게 점검하고, 연결 문제를 진단할 때 유용합니다.

## Layer 3 Forwarding

**Layer 3 포워딩**은 다음 두 가지 시나리오를 처리합니다:
- 같은 **서브넷** 간 트래픽 전달
- 다른 **서브넷** 간 트래픽 라우팅

OSI 계층 기준으로, 데이터는 **Layer 7**에서 시작하여 **Layer 1**로 내려가며 캡슐화되어 전송됩니다.

---

### Local Network Forwarding

같은 서브넷에 있는 경우:
- 송신 장치는 **자신의 MAC 주소는 알고 있지만**, 목적지 MAC 주소는 모를 수 있습니다.
- 이를 해결하기 위해 **ARP (Address Resolution Protocol)** 사용

---

## Key Topic: ARP (Address Resolution Protocol)

**ARP**는 IP 주소를 MAC 주소에 매핑하기 위해 사용됩니다.

1. 송신자는 **브로드캐스트 ARP 요청**을 전송합니다.
2. 대상 IP를 가진 장치는 **유니캐스트 ARP 응답**을 반환합니다.

---

### ARP 테이블 조회 명령어

```shell
show ip arp [mac-address | ip-address | vlan <vlan-id> | interface <interface-id>]
Packet Routing
```
다른 서브넷의 IP 주소로 패킷을 전송할 경우:
장치는 라우팅 테이블과 ARP 테이블을 조회

**다음 홉(Next Hop)**의 IP 주소 및 MAC 주소를 확인하여 전송

---

### Figure 1-5: Layer 2 Addressing Rewrite
예: PC-A → R1 → PC-B

R1을 경유하면서 목적지 MAC 주소가 변경됨

라우터는 Layer 3 주소(IP)를 유지하면서 Layer 2 주소(MAC)를 수정하여 포워딩함

IPv4 주소 할당
라우터 인터페이스에 IPv4 주소를 할당하려면:

```
ip address <ip-address> <subnet-mask>
```
인터페이스가 up 상태일 경우, 해당 네트워크는 자동으로 **Routing Information Base (RIB)**에 등록됩니다.

RIB에 등록된 직접 연결된 네트워크의 Administrative Distance는 0으로, 가장 높은 우선순위를 가집니다.

보조 IP 주소 (Secondary IP)
하나의 인터페이스에 여러 IPv4 주소를 할당할 수 있으며, 이 경우 secondary 키워드를 사용합니다.

IPv6 주소 할당
```
ipv6 address <ipv6-address>/<prefix-length>
```

---

### Example 1-11: Assigning IP Addresses to Routed Interfaces
```
R1(config)# configure terminal
R1(config)# interface gi0/0/0
R1(config-if)# ip address 10.10.10.254 255.255.255.0
R1(config-if)# ip address 172.16.10.254 255.255.255.0 secondary
R1(config-if)# ipv6 address 2001:db8:10::254/64
R1(config-if)# ipv6 address 2001:db8:20::172:1/64
Routed Subinterfaces
```

VLAN 간 라우팅을 위한 **서브인터페이스 (Subinterface)**는
물리 포트를 절약하면서 여러 VLAN에 대해 라우팅을 수행할 수 있게 합니다.

서브인터페이스 구성 방식
```
interface <interface-name>.<subinterface-number>
encapsulation dot1Q <vlan-id>
```
---

### Example 1-12: Configuring Routed Subinterfaces
```
R2(config)# interface g0/0/1.10
R2(config-subif)# encapsulation dot1Q 10
R2(config-subif)# ip address 10.10.10.2 255.255.255.0
R2(config-subif)# ipv6 address 2001:db8:10::2/64
```
서브인터페이스는 각 VLAN ID에 맞춰 encapsulation 설정 후, IP 주소를 개별 할당합니다.

---

## Switched Virtual Interfaces (SVIs)

**멀티레이어 스위치**는 VLAN 인터페이스(SVI: Switched Virtual Interface)에 **IP 주소를 설정하여 라우팅** 기능을 수행할 수 있습니다.

- 각 SVI는 해당 **VLAN이 활성화되어 있어야** 동작합니다.
- SVI는 **인터페이스 vlan <vlan-id>** 명령을 통해 생성됩니다.

### Example 1-13: Creating a Switched Virtual Interface (SVI)

```shell
SW1(config)# interface vlan 10
SW1(config-if)# ip address 10.10.10.1 255.255.255.0
SW1(config-if)# ipv6 address 2001:db8:10::1/64
SW1(config-if)# no shutdown
```
SVI는 인터페이스 상태가 up, 그리고 해당 VLAN이 스위치에 존재해야 활성화됩니다.

Routed Switch Ports
스위치의 Layer 2 포트를 Layer 3 포트로 변경하려면 no switchport 명령을 사용합니다.
라우터처럼 직접 IP 주소를 할당하여 Layer 3 인터페이스로 동작하게 만듭니다.

### Example 1-14: Configuring a Routed Switch Port
```
SW1(config)# interface gi1/0/14
SW1(config-if)# no switchport
SW1(config-if)# ip address 10.20.20.1 255.255.255.0
SW1(config-if)# ipv6 address 2001:db8:20:1::1/64
SW1(config-if)# no shutdown
```

IP 주소 검증 명령어
IPv4 확인
```
show ip interface [brief | interface-id | vlan <vlan-id>]
```
- 할당된 IPv4 주소
- 인터페이스 상태 (up/down)
- 프로토콜 상태를 요약

IPv6 확인
```
show ipv6 interface [brief | interface-id | vlan <vlan-id>]
```

IPv6 주소 및 상태를 IPv4와 동일한 방식으로 확인 가능

Forwarding Architectures (전달 아키텍처)
발전 과정 요약
초기 라우터는 CPU를 이용한 패킷 처리 (소프트웨어 처리)
현재는 Layer 2 헤더를 제거하지 않고, 빠르게 IP 처리 수행
빠른 전송을 위해 다양한 포워딩 기법이 도입됨

Key Topic: Process Switching
Process Switching은 가장 기본적인 소프트웨어 기반 포워딩 방식입니다.

다음 상황에서 사용됩니다:

- 라우터 자체가 목적지인 패킷
- 하드웨어로 처리할 수 없는 복잡한 패킷
- ARP 해결되지 않은 상태의 패킷
- ip_input 프로세스
- 라우팅 테이블과 ARP 테이블을 참조하여 목적지 MAC 주소 결정
- TTL 감소, IP 체크섬 재계산 수행
- 다음 홉 라우터로 전달

---

### Figure 1-6: Process Switching 흐름
하드웨어 전송이 불가능한 패킷이 **소프트웨어 경로(ip_input)**로 전달되는 흐름을 도식화

CEF로 넘기지 못한 예외 상황 처리에 사용됨
Process Switching은 느리지만 예외 처리, 진단 및 디버깅 시 필수적인 포워딩 방식입니다.
