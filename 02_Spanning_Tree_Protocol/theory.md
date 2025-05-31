# CHAPTER 2  
## Spanning Tree Protocol

이 장에서는 다음과 같은 주제를 다룹니다:

- **Spanning Tree Protocol Fundamentals**: 이 섹션은 스위치가 다른 스위치를 인식하고 전달 루프를 방지하는 방법에 대한 개요를 제공합니다.
- **Rapid Spanning Tree Protocol**: 이 섹션은 더 빠른 수렴을 위해 STP에 적용된 개선 사항을 살펴봅니다.

좋은 네트워크 설계는 장치 및 네트워크 링크(즉, 경로)의 중복성을 제공합니다. 가장 간단한 해결책은 네트워크 링크 장애를 극복하거나 스위치가 토폴로지 내에서 최소 두 개 이상의 다른 스위치에 연결되어 있도록 하는 것입니다.

그러나 이러한 토폴로지는 브로드캐스트 또는 알려지지 않은 유니캐스트 플러딩(unknown unicast flooding) 트래픽을 스위치가 전달해야 할 경우 문제가 발생할 수 있습니다. 네트워크 브로드캐스트는 루프를 통해 연속적으로 전달되어 링크가 포화되고 스위치는 패킷을 드롭하게 됩니다. 또한 MAC 주소 테이블은 패킷이 루프를 도는 동안 지속적으로 변경되어 토폴로지가 안정되지 않게 됩니다.

이러한 루프는 Layer 2 포워딩에 TTL(Time to Live) 메커니즘이 없기 때문에 발생합니다. 스위치의 CPU 사용률이 증가하고, 메모리 소비도 증가하여 결국 스위치가 다운될 수 있습니다.

이 장에서는 스위치가 어떻게 전달 루프를 방지하면서도 중복 링크를 허용하는지를 **Spanning Tree Protocol(STP)** 및 **Rapid Spanning Tree Protocol(RSTP)** 을 사용하여 설명합니다.

이외에도 두 개의 추가 장이 STP 관련 주제를 설명합니다:

- Chapter 3, “Advanced STP Tuning”: BPDU guard 및 BPDU 필터와 같은 고급 STP 주제를 다룹니다.
- Chapter 4, “Multiple Spanning Tree Protocol”: MST(Multiple Spanning Tree Protocol)을 다룹니다.

---

## “Do I Know This Already?” Quiz

“Do I Know This Already?” 퀴즈는 독자가 해당 장 전체를 읽을 필요가 있는지 여부를 판단할 수 있도록 도와줍니다. 이 셀프 테스트 질문 중 하나 이상을 틀리면, **"Exam Preparation Tasks"** 섹션으로 넘어가기 전에 이 장 전체를 읽는 것이 좋습니다.

### Table 2-1: Foundation Topics Section-to-Question Mapping

| Foundation Topics Section         | Questions |
|----------------------------------|-----------|
| Spanning Tree Protocol Fundamentals | 1–6     |
| Rapid Spanning Tree Protocol     | 7–9       |

---

### Quiz 문항

1. **BPDU 유형은 총 몇 가지가 있나요?**  
   a. 하나  
   b. 두 개 ✅  
   c. 세 개  
   d. 네 개  
   **해설**: Configuration BPDU와 Topology Change Notification (TCN) BPDU 두 가지가 있습니다 (p.40 참조).

2. **루트 브리지를 선출하는 데 사용되는 주요 속성은 무엇인가요?**  
   a. 스위치 포트 우선순위  
   b. 브리지 우선순위 ✅  
   c. 스위치 일련번호  
   d. 경로 비용  
   **해설**: 브리지 우선순위가 낮을수록 루트 브리지로 선택될 가능성이 높습니다. (동일한 경우 MAC 주소 기준) (p.42).

3. **원래 802.1D 명세는 1Gbps 인터페이스에 어떤 포트 비용 값을 할당하나요?**  
   a. 1  
   b. 2  
   c. 4 ✅  
   d. 19  
   **해설**: 표 2-2 참조. 1Gbps는 Short-Mode 기준 포트 비용 4 (p.41).

4. **루트 브리지에 있는 모든 포트의 역할은 무엇인가요?**  
   a. 루트 포트  
   b. 지정 포트 ✅  
   c. 상위 포트  
   d. 마스터 포트  
   **해설**: 루트 브리지는 전송을 위한 모든 포트가 지정 포트로 설정됩니다 (p.39).

5. **기본 설정에서, 포트가 Listening 상태에 머무는 시간은 얼마인가요?**  
   a. 2초  
   b. 5초  
   c. 10초  
   d. 15초 ✅  
   **해설**: Forward delay 타이머는 기본적으로 15초입니다 (p.40).

6. **토폴로지 변경 플래그가 설정된 설정 BPDU를 수신할 때, 하위 스위치는 어떻게 반응하나요?**  
   a. 모든 포트를 차단 상태로 이동시킨다.  
   b. MAC 주소 테이블에서 모든 MAC 주소를 제거한다. ✅  
   c. 모든 비루트 포트를 일시적으로 Listening 상태로 이동시킨다.  
   d. MAC 주소 테이블에서 오래된 MAC 주소만 제거한다.  
   e. 로컬 스위치 데이터베이스에 토폴로지 변경 플래그 버전을 업데이트한다.  
   **해설**: 토폴로지 변경 시 모든 MAC 주소를 제거하여 다시 학습합니다 (p.40).

7. **다음 중 RSTP 포트 상태가 아닌 것은 무엇인가요?**  
   a. Blocking ✅  
   b. Listening  
   c. Learning  
   d. Forwarding  
   **해설**: RSTP에서는 Listening 상태가 존재하지 않습니다 (p.38).

8. **참/거짓: 802.1D와 비교했을 때 RSTP는 포트 상태 수를 5개에서 4개로 줄인다.**  
   a. 참 ✅  
   b. 거짓  
   **해설**: Disabled, Blocking, Listening, Learning, Forwarding → RSTP는 이를 간소화합니다 (p.38).

9. **참/거짓: RSTP를 실행 중인 대규모 L2 토폴로지에서는, 패킷이 전달되기 전에 반드시 전체 인프라가 완전히 수렴해야 한다.**  
   a. 참  
   b. 거짓 ✅  
   **해설**: RSTP는 빠른 수렴을 지원하며, 전체 인프라가 수렴될 때까지 기다릴 필요는 없습니다 (p.38).

## Foundation Topics  
### Spanning Tree Protocol Fundamentals

**Spanning Tree Protocol (STP)** 은 스위치가 다른 스위치에 대한 인식을 가능하게 하여 전달 루프를 방지할 수 있도록 합니다. 이는 **브리지 프로토콜 데이터 유닛(BPDU)** 의 전송 및 수신을 통해 이루어집니다. STP는 중복 포트를 임시로 차단하여 루프 없는 레이어 2 토폴로지를 구축합니다. 이는 최적의 전송 경로를 선택하고 트리 기반 알고리즘을 실행하여 중복 포트가 트래픽을 전달하지 않도록 합니다.

#### STP 구현 버전

- **802.1D**: STP의 원래 명세
- **Per-VLAN Spanning Tree (PVST)**
- **Per-VLAN Spanning Tree Plus (PVST+)**
- **802.1w Rapid Spanning Tree Protocol (RSTP)**
- **802.1s Multiple Spanning Tree Protocol (MST)**

> Catalyst 스위치는 PVST+, RSTP, MST 모드로 작동하며, 이 세 가지 모드는 모두 802.1D와 호환됩니다.

---

### IEEE 802.1D STP

STP의 원래 버전은 IEEE 802.1D 표준에서 유래되었으며, 하나의 VLAN에 대해 루프 없는 토폴로지를 보장합니다. 이 주제는 **Rapid Spanning Tree Protocol (RSTP)** 과 **Multiple Spanning Tree Protocol (MST)** 의 기반이 되므로 매우 중요합니다.

---

### 802.1D 포트 상태

802.1D STP에서 모든 포트는 다음 상태들을 통해 전이됩니다:

- **Disabled**: 포트가 관리적으로 비활성화된 상태입니다 (예: `shutdown`).
- **Blocking**: 스위치 포트는 활성화되어 있지만, 트래픽을 전달하지 않으며 루프 방지를 위해 사용됩니다. 이 상태에서 MAC 주소 테이블은 수정되지 않고, BPDU 수신만 가능합니다.
- **Listening**: 포트가 Blocking에서 전이되어 BPDU 수신은 가능하지만 트래픽 전달은 불가능합니다. 이 상태의 지속 시간은 STP의 전이 시간(포워딩 시간)에 따라 결정됩니다.
- **Learning**: 포트가 수신한 트래픽을 기반으로 MAC 주소 테이블을 수정할 수 있지만, 여전히 트래픽 전달은 하지 않습니다.
- **Forwarding**: 포트가 전체 트래픽을 전달하며 MAC 주소 테이블도 업데이트합니다. 이는 최종 포워딩 상태입니다.
- **Broken**: 구성 오류나 운영상의 문제로 인해 발생하며, 문제가 해결될 때까지 패킷을 폐기합니다.

> 참고: 전체 STP 초기화에는 약 **30초**가 걸리며, 기본 타이머를 사용할 경우 Blocking에서 Forwarding으로 전이하는 데 필요한 시간입니다.

---

### 802.1D 포트 유형

802.1D 표준은 다음과 같은 **3가지 포트 유형**을 정의합니다:

- **Root Port (RP)**: 루트 브리지 또는 상위 스위치에 연결되는 네트워크 포트입니다. VLAN당 하나의 루트 포트만 존재해야 합니다.
- **Designated Port (DP)**: 다른 스위치에 BPDU 프레임을 전달하는 포트입니다. 링크당 단 하나의 활성 지정 포트만 있어야 합니다.
- **Blocking Port**: STP 계산 결과에 따라 트래픽을 전달하지 않는 포트입니다.

---

### STP 주요 용어

- **Root Bridge**: Layer 2 토폴로지에서 가장 중요한 스위치입니다. 모든 포트가 포워딩 상태입니다. 이 스위치는 스패닝 트리 계산의 기준점이며, 다른 모든 스위치는 루트 브리지로부터의 경로 비용을 기준으로 계산을 수행합니다. 루트 브리지의 모든 포트는 **지정 포트(Designated Port)** 로 분류됩니다.

### Bridge Protocol Data Unit (BPDU)

**BPDU**는 네트워크 스위칭 환경에서 계층 구조를 식별하고 토폴로지 변경 사항을 알리는 데 사용되는 네트워크 패킷입니다.  
BPDU는 MAC 주소 `01:80:c2:00:00:00` 을 사용하며, **두 가지 유형**이 있습니다:

#### • Configuration BPDU
이 BPDU는 루트 브리지, 루트 포트, 지정 포트 및 차단 포트를 식별하는 데 사용됩니다.  
다음과 같은 필드를 포함합니다:

- STP 타입  
- 루트 경로 비용 (Root Path Cost)  
- 루트 브리지 식별자 (Root Bridge ID)  
- 로컬 브리지 식별자 (Local Bridge ID)  
- Max Age  
- Hello Time  
- Forward Delay

#### • Topology Change Notification (TCN) BPDU
이 BPDU는 Layer 2 토폴로지 변경 사항을 다른 스위치에 알리기 위해 사용됩니다.

---

### Root Path Cost

**Root Path Cost**는 루트 스위치까지 특정 경로에 대한 누적 비용입니다.

- **System Priority**: 루트 브리지를 선출할 때 사용되는 4비트 우선순위. 기본값은 `32,768`
- **System ID Extension**: VLAN ID를 나타내는 12비트 값으로, 브리지 ID에 포함됨
- **Local Bridge Identifier**: MAC 주소 + System Priority + System ID Extension의 조합
- **Max Age**: 브리지가 BPDU 정보를 저장하는 최대 시간 (기본값: `20초`)
- **Hello Time**: 포트에서 BPDU를 전송하는 주기 (기본값: `2초`)
- **Forward Delay**: 포트가 Listening 및 Learning 상태에 머무는 시간. 기본값은 `15초`이며, 총 30초가 필요

> 참고: STP는 스위치보다 먼저 존재한 프로토콜입니다.  
> 초기에는 이 기술을 사용하는 장비들이 **"브리지(Bridge)"** 로 불렸고, 현재의 **스위치(Switch)** 와 같은 역할을 했습니다.  
> 이 문맥에서 **bridge**와 **switch**는 동일하게 사용됩니다.

---

### Spanning Tree Path Cost

**STP 경로 비용**은 루트 브리지까지의 누적 STP 인터페이스 비용입니다. 이 비용은 다음 기준에 따라 측정됩니다:

- 원래는 **16비트 값**으로 저장되며 기준 대역폭은 **20Gbps**
- 최신 방식인 **long mode**는 **32비트 값**을 사용하며 기준 대역폭은 **20Tbps**
- 대부분의 스위치에서는 이제 기본값이 **short mode**이며, 필요에 따라 **long mode**로 변경 가능

## 표 2-2: STP 포트 비용 (Short / Long 모드)

| 링크 속도  | Short-Mode STP 비용 | Long-Mode STP 비용 |
|------------|----------------------|---------------------|
| 10 Mbps    | 100                  | 2,000,000           |
| 100 Mbps   | 19                   | 200,000             |
| 1 Gbps     | 4                    | 20,000              |
| 10 Gbps    | 2                    | 2,000               |
| 20 Gbps    | 1                    | 1,000               |
| 100 Gbps   | 1                    | 200                 |
| 1 Tbps     | 1                    | 20                  |
| 10 Tbps    | 1                    | 2                   |

---

## STP 토폴로지 구축

STP의 첫 번째 단계는 **루트 브리지(Root Bridge)** 를 식별하는 것입니다.  
각 스위치는 처음에는 자신이 루트 브리지라고 가정하고 자신의 **로컬 브리지 ID** 를 사용하여 구성 BPDU를 전송합니다.

이후 인접한 스위치의 구성 BPDU를 수신하면서 다음과 같은 판단을 합니다:

- 인접 스위치의 BPDU가 더 나쁘면, 자신의 BPDU를 유지  
- 인접 스위치의 BPDU가 더 낫다면, 자신의 BPDU를 업데이트하고 새로운 루트 브리지 ID 및 누적 루트 경로 비용을 반영하여 BPDU를 다시 전송

> 이 과정은 토폴로지 내의 모든 스위치가 루트 브리지를 인식할 때까지 반복됩니다.

---

### Figure 2-1: 기본 STP 토폴로지

- STP는 브리지 식별자의 **우선순위가 낮을수록** 선호됨
- BPDU의 우선순위가 같다면 **MAC 주소가 더 낮은 스위치**가 루트 브리지로 선정됨
- 일반적으로 오래된 스위치는 MAC 주소가 낮아 루트 브리지로 선호됨
- 구성 변경이 없다면 기존 스위치가 루트 브리지를 유지하고, 새로운 스위치가 연결되어도 루트 브리지로 승격되지 않음

#### 예시:

- SW1이 루트 브리지로 식별됨
- MAC 주소 `0062.ec9d.c500` 이 네트워크 내에서 가장 낮음
- `show spanning-tree root` 명령으로 SW1의 루트 브리지 상태 확인 가능

---

### Example 2-1: STP 루트 브리지 확인

```bash
SW1# show spanning-tree root

VLAN     Root ID               Cost  Hello Max Fwd  Root Port
-------- -------------------- ----- ------ -------- ----------
VLAN0001 32769 0062.ec9d.c500   1      2     20    15   —
VLAN0010 32778 0062.ec9d.c500   1      2     20    15   —
VLAN0020 32788 0062.ec9d.c500   1      2     20    15   —
VLAN0099 32867 0062.ec9d.c500   1      2     20    15   —
```

---

# 루트 경로 비용 계산

SW1의 우선순위는 VLAN 1에서 **32,769**이며, 구성 BPDU의 우선순위는 VLAN ID가 더해져 **sys-id-ext 필드**로 표현됩니다.

BPDU를 생성할 때 **루트 포트에 대해 비용을 계산**하지만, **자신의 포트 비용은 포함하지 않습니다**.

- BPDU가 **발송된 포트의 비용은 제외**
- BPDU를 **수신한 포트의 비용만 누적**
- 따라서 **루트 브리지의 루트 경로 비용은 항상 0**

---

## [Figure 2-2] STP 경로 비용 광고

이 그림은 **SW1에서 SW3까지의 루트 경로 비용 누적**을 설명합니다:

- SW1 → SW3로 BPDU가 전달됨
- SW2와 SW3는 **1Gbps 링크**를 사용하며, 각각의 **포트 비용은 4**
- 이로 인해 루트 경로 비용은 **총 8**
- `**Gi1/0/1**` 포트는 **루트 포트(RP)**로 식별됨

---

### Example 2-2: 루트 포트 식별 (SW2, SW3)

각 스위치에서 `show spanning-tree root` 명령을 실행하여 루트 브리지 정보 및 루트 포트를 확인할 수 있습니다.

```bash
SW2# show spanning-tree root
VLAN     Root ID               Cost  Hello Max Fwd  Root Port
-------- -------------------- ----- ------ -------- ----------
VLAN0001 32769 0062.ec9d.c500   4      2     20      Gi1/0/1
SW2는 루트 브리지의 Bridge ID: 32769 0062.ec9d.c500를 인식함

루트 포트는 Gi1/0/1이며, 경로 비용(Cost)은 4

1Gbps 링크를 통해 루트 브리지(SW1)에 도달하고 있음


SW3# show spanning-tree root
VLAN     Root ID               Cost  Hello Max Fwd  Root Port
-------- -------------------- ----- ------ -------- ----------
(동일 출력 생략)

## 🧭 루트 포트(RP) 식별 기준

스위치는 루트 브리지를 인식한 후, **각 포트에서 수신한 BPDU 정보를 기준으로 루트 포트(Root Port)**를 식별합니다.
루트 포트는 스위치가 루트 브리지까지 가장 효율적으로 도달할 수 있는 경로를 의미합니다.

### 루트 포트 선정 기준 순서:

1. **가장 낮은 경로 비용(Path Cost)을 가진 포트**
2. **가장 낮은 시스템 우선순위(System Priority)**를 가진 광고 스위치
3. **가장 낮은 MAC 주소**를 가진 광고 스위치
4. 동일 스위치 내에서 여러 링크가 있을 경우, **포트 우선순위가 낮은 포트**
5. 포트 우선순위가 동일하다면, **포트 번호가 낮은 포트**
```
---

### Example 2-3: SW4 및 SW5에서 루트 포트 식별

```
SW4# show spanning-tree root
VLAN0001 ... Cost 8 ... Root Port Gi1/0/2

SW5# show spanning-tree root
VLAN0001 ... Cost 8 ... Root Port Gi1/0/3

SW4의 루트 포트: Gi1/0/2

SW5의 루트 포트: Gi1/0/3

두 스위치 모두 루트 경로 비용(Cost)은 8

💡 루트 경로 비용이 8인 이유:
→ 두 스위치(SW4, SW5)는 1Gbps 링크를 두 번 거쳐 루트 브리지 SW1에 도달하기 때문입니다.
(1Gbps 링크의 STP 포트 비용은 4 → 4 + 4 = 8)
```
---

## 차단된 지정 포트 (Blocked Designated Switch Ports) 찾기

루트 브리지와 루트 포트(RP)가 식별되었다면, **나머지 모든 포트는 지정 포트(Designated Port)**로 간주됩니다.  

그러나 두 개의 **루트가 아닌(non-root) 스위치**가 서로 **직접 연결된 경우**, **스위치 루프 방지를 위해 한쪽 포트는 반드시 차단(Blocked) 상태**로 전환되어야 합니다.

### 예제 토폴로지에서 차단이 필요한 링크:

- SW2 `Gi1/0/3` ↔ SW3 `Gi1/0/2`
- SW4 `Gi1/0/5` ↔ SW5 `Gi1/0/4`
- SW4 `Gi1/0/6` ↔ SW5 `Gi1/0/5`

---

## 차단 포트 선택 로직

두 개의 비루트 스위치가 서로 연결되어 있을 경우, 어떤 포트를 차단할지 결정하는 기준은 다음과 같습니다:

1. 포트가 **지정 포트(Designated Port)**인지 확인  
   → 루트 포트는 차단 대상에서 **제외**
2. **루트 브리지까지의 경로 비용이 높은 쪽**을 차단
3. 경로 비용이 동일하다면, **시스템 우선순위(System Priority)**가 더 높은 쪽 차단
4. 우선순위도 동일하다면, **MAC 주소가 더 높은 쪽을 차단**

### 예시:

- SW3의 `Gi1/0/2`, SW5의 `Gi1/0/4`, SW4의 `Gi1/0/5`는 모두 **루트 포트가 아님**
- 따라서 각 링크에서 **MAC 주소가 더 높은 스위치의 포트가 차단됨**

---

## 명령어: `show spanning-tree [vlan vlan-id]`

이 명령어는 **STP 상태 확인**에 유용합니다.  
예제 2-4에서는 VLAN 1에 대해 **SW1의 STP 상태**를 출력합니다.

출력 정보:

- 루트 브리지 정보
- 로컬 브리지 정보
- 인터페이스의 STP 포트 비용, 포트 우선순위, 포트 역할

💡 **SW1의 모든 포트는 `Desg(Designated)` 상태**  
→ SW1이 **루트 브리지**이므로 모든 포트가 지정 포트로 동작

---

## Catalyst 스위치의 포트 유형

| 포트 유형 | 설명 |
|-----------|------|
| **Point-to-Point (P2P)** | 다른 네트워크 장치(PC 또는 RSTP 스위치)와 연결된 포트 |
| **P2P Edge**             | **PortFast**가 설정된 포트 (일반적으로 사용자 단말에 연결됨) |

---

## Rapid Spanning Tree Protocol (RSTP)

**RSTP (IEEE 802.1w)**는 전통적인 STP(802.1D)에 비해 **더 빠른 수렴 속도**를 제공합니다.

- STP는 포트가 포워딩 상태로 전환되기까지 **최대 30초 이상**이 소요될 수 있음
- RSTP는 **몇 초 내에 포트 상태 전환** 가능

---

### RSTP 포트 상태

RSTP는 STP보다 단순화된 **3가지 포트 상태**만 사용합니다:

| 상태       | 설명 |
|------------|------|
| **Discarding** | 트래픽 전달하지 않음 (STP의 Blocking + Listening에 해당) |
| **Learning**   | MAC 주소 학습은 가능, 트래픽 전달은 불가 |
| **Forwarding** | 트래픽 전달 가능 |

✅ STP의 5가지 상태(Disabled, Blocking, Listening, Learning, Forwarding) 중  
**Listening** 상태가 제거되어 단순화됨  
➡ **퀴즈 문항 7, 8 참조**

---

### RSTP 포트 역할

RSTP는 다음과 같은 포트 역할을 정의합니다:

- **Root Port (RP)**: 루트 브리지로 가는 최단 경로
- **Designated Port (DP)**: 세그먼트 내 트래픽 전달 담당
- **Alternate Port**: RP 장애 시 사용되는 백업 경로
- **Backup Port**: 동일 스위치 내 다른 포트와 세그먼트를 공유하며 백업 역할 수행
- **Edge Port**: PortFast가 설정된 포트 (PC 등 종단 장치 연결)

💡 RSTP는 이러한 **포트 역할에 기반해 루프를 빠르게 차단하거나 포워딩을 재구성**할 수 있음

## RSTP의 주요 이점

**Rapid Spanning Tree Protocol (RSTP)**는 기존 STP 대비 다음과 같은 향상된 기능을 제공합니다:

-  **빠른 수렴 시간**  
  링크 장애 발생 시 루트 포트(RP) 또는 지정 포트(DP)를 **빠르게 재선정**할 수 있습니다.
  
-  **Edge Port 인식**  
  종단 장비(PC 등)에 연결된 **Edge 포트는 지연 없이 즉시 포워딩 상태로 전환**됩니다.  
  (PortFast 기능이 기본 내장)

-  **BPDU 상호작용 향상**  
  STP와 달리 **모든 포트가 BPDU를 주기적으로 전송**하고 수신합니다.  
  이로 인해 토폴로지 변화에 **더 빠르게 대응**할 수 있습니다.

 **RSTP는 STP와 완전 호환**됩니다.  
즉, RSTP 스위치는 **STP 스위치와도 문제 없이 상호 운용**할 수 있으며,  
단 **양쪽 장비 모두 RSTP를 지원**해야 모든 기능을 온전히 사용할 수 있습니다.

---

## 정리 요약

| 항목 | 설명 |
|------|------|
| **STP (802.1D)** | 느리지만 표준 기반의 안정적인 스패닝 트리 프로토콜 |
| **RSTP (802.1w)** | STP를 대체하며 **빠른 회복**과 **간소화된 포트 역할/상태** 제공 |
| **포트 상태/역할** | 5개 상태 → 3개 상태로 단순화, 역할은 더 다양화 |
| **Edge 기능** | PortFast가 기본 통합되어 **종단 장비 연결 시 즉시 포워딩 가능** |
| **운영 모드** | 대부분의 최신 Catalyst 스위치는 **RSTP 모드 또는 PVST+ RSTP 모드**에서 운영 가능 |

## Example 2-4: SW1의 STP 정보 보기

```bash
SW1# show spanning-tree vlan 1
VLAN0001
 Spanning tree enabled protocol rstp

 This section displays the relevant information for the STP root bridge
 Root ID  Priority    32769
          Address     0062.ec9d.c500
          This bridge is the root
          Hello Time 2 sec  Max Age 20 sec  Forward Delay 15 sec

 This section displays the relevant information for the Local STP bridge
 Bridge ID  Priority    32769 (priority 32768 sys-id-ext 1)
            Address     0062.ec9d.c500
            Hello Time 2 sec  Max Age 20 sec  Forward Delay 15 sec
            Aging Time 300 sec

 Interface       Role Sts Cost     Prio.Nbr Type
 Gi1/0/2         Desg FWD 4        128.2     P2p
 Gi1/0/3         Desg FWD 4        128.3     P2p
 Gi1/0/14        Desg FWD 4        128.14    P2p Edge

🧾 분석
SW1은 루트 브리지로 동작 중 (Root ID = Bridge ID)

모든 인터페이스는 **Designated 포트(Desg)**로, 상태는 Forwarding(FWD)

포트 비용은 모두 4 (1Gbps 링크 기준)

포트 역할 및 상태는 다음과 같음:

인터페이스	역할 (Role)	상태 (Sts)	비용 (Cost)	우선순위.번호 (Prio.Nbr)	타입 (Type)
Gi1/0/2	Desg	FWD	4	128.2	P2p
Gi1/0/3	Desg	FWD	4	128.3	P2p
Gi1/0/14	Desg	FWD	4	128.14	P2p Edge

💡 참고:
Prio.Nbr 항목에 .1, .2, .14 등 소수점 포함된 값은
포트 우선순위 + 포트 번호를 의미합니다.
Trunk/Access 모드 설정 오류나 포트 타입 설정 문제의 단서가 될 수 있으므로 주의해서 확인해야 합니다.
```

## Example 2-5: VLAN에서 루트 및 차단 포트 확인

아래는 SW2와 SW3의 VLAN 1에 대한 RSTP 상태를 확인하는 명령어 출력입니다.

---

### SW2 - `show spanning-tree vlan 1`

```bash
SW2# show spanning-tree vlan 1
VLAN0001
 Spanning tree enabled protocol rstp

 Root ID    Priority 32769
            Address  0062.ec9d.c500
            Cost     4
            Port     1 (GigabitEthernet1/0/1)
            Hello Time 2 sec  Max Age 20 sec  Forward Delay 15 sec

 Bridge ID  Priority 32769 (priority 32768 sys-id-ext 1)
            Address  0081.c4ff.8b00
            Hello Time 2 sec  Max Age 20 sec  Forward Delay 15 sec
            Aging Time 300 sec

 Interface       Role Sts Cost     Prio.Nbr Type
 ---------       ---- --- ----     -------- ----
 Gi1/0/1         Root FWD 4        128.1     P2p
 Gi1/0/3         Desg FWD 4        128.3     P2p
 Gi1/0/2         Desg FWD 4        128.2     P2p
**SW2는 루트 브리지(SW1)**까지의 경로 비용이 4이며, 루트 포트는 Gi1/0/1

모든 포트는 포워딩(FWD) 상태
```

### SW3# show spanning-tree vlan 1
```
VLAN0001
 Spanning tree enabled protocol rstp

 Root ID    Priority 32769
            Address  0062.ec9d.c500
            Cost     4
            Port     1 (GigabitEthernet1/0/1)
            Hello Time 2 sec  Max Age 20 sec  Forward Delay 15 sec

 Bridge ID  Priority 32769 (priority 32768 sys-id-ext 1)
            Address  189c.5d11.9980
            Hello Time 2 sec  Max Age 20 sec  Forward Delay 15 sec
            Aging Time 300 sec

 Interface       Role Sts Cost     Prio.Nbr Type
 ---------       ---- --- ----     -------- ----
 Gi1/0/1         Root FWD 4        128.1     P2p
 Gi1/0/2         Altn BLK 4        128.2     P2p
 Gi1/0/5         Desg FWD 4        128.5     P2
**SW3의 루트 포트는 Gi1/0/1**이며, 루트까지의 비용은 동일하게 4

Gi1/0/2 포트는 Alternate(Altn) 역할이며, Blocked(차단) 상태

🧠 차단 사유
SW2와 SW3는 Gi1/0/3 ↔ Gi1/0/2로 상호 연결됨

루트 브리지까지의 경로 비용은 동일 (4)

MAC 주소 우선순위 비교 결과:

SW2: 0081.c4ff.8b00

SW3: 189c.5d11.9980

따라서 MAC 주소가 더 높은 SW3의 포트(Gi1/0/2)가 차단됨
```

## 🔎 Example 2-6: 트렁크 링크에서 STP 상태 확인

다중 VLAN이 설정된 트렁크 링크(SW3의 `Gi1/0/1`)에서 각 VLAN의 STP 상태를 확인하는 예제입니다.

---

### `show spanning-tree interface gi1/0/1`

```bash
SW3# show spanning-tree interface gi1/0/1
Vlan         Role Sts Cost  Prio.Nbr Type
VLAN0001     Root FWD 4     128.1     P2p
VLAN0010     Root FWD 4     128.1     P2p
VLAN0020     Root FWD 4     128.1     P2p
VLAN0099     Root FWD 4     128.1     P2p
Gi1/0/1 포트는 VLAN 1, 10, 20, 99에 대해 모두 루트 포트(Root)이며 Forwarding 상태

각 VLAN은 동일한 포트 우선순위(128.1)와 비용(4)을 가짐

연결 유형은 모두 Point-to-Point (P2p)
```

### `show spanning-tree interface gi1/0/1 detail`

```
SW3# show spanning-tree interface gi1/0/1 detail
Port 1 (GigabitEthernet1/0/1) of VLAN0001 is root forwarding
Port path cost 4, Port priority 128, Port Identifier 128.1

...
BPDU: sent 15, received 22957
주요 정보 분석:
항목	내용
포트 상태	VLAN 1에서 Root Forwarding
경로 비용 (Cost)	4 (1Gbps 링크 기준)
포트 우선순위	128
포트 ID	128.1
BPDU 송/수신	송신: 15회 / 수신: 22,957회

💡 트렁크 링크의 경우, 포트 하나가 여러 VLAN에서 STP 역할을 개별적으로 수행합니다.
각 VLAN에 대해 별도의 STP 인스턴스가 적용되며, 포트 역할과 상태도 VLAN별로 분리 관리됩니다.
```

## 🔁 STP 토폴로지 변경

안정적인 Layer 2 토폴로지에서는 **루트 브리지로부터 가장자리 스위치까지 구성 BPDU가 흐릅니다**.  
그러나 **링크 다운 등의 변경이 발생하면**, **Topology Change Notification(TCN) BPDU**가 전송되어 전체 네트워크에 영향을 줍니다.

### 📌 토폴로지 변경 발생 조건 및 반응

- 루트 포트가 다운되면, 해당 스위치가 **TCN 전송**
- 루트 브리지가 이를 수신하면 **구성 BPDU에 토폴로지 변경 플래그 설정**
- 다른 스위치들은 **MAC 주소 테이블의 aging 타이머를 15초로 단축**
- 오래된 MAC 엔트리를 제거하지만 **통신 중인 장치는 유지**

---

## 🔍 Example 2-7: `show spanning-tree vlan 10 detail`

```bash
SWx# show spanning-tree vlan 10 detail
...
Number of topology changes 3 last change occurred 01:02:09 ago
 from GigabitEthernet1/0/2
마지막 변경은 1시간 2분 9초 전

Gi1/0/2 포트에서 토폴로지 변경 발생
```

## 🔌 Converging with Direct Link Failures

스위치가 전원을 잃거나 재부팅하거나, 케이블이 제거되는 경우 **Layer 1 신호가 소실**되고 포트는 다운 상태가 됩니다.  
이러한 상태를 STP는 **링크 장애(link failure)**로 인식하여 **토폴로지를 재계산하고 경로를 재구성**합니다.

---

### 📘 직접 링크 장애 시나리오 1: SW2 ↔ SW3 링크 다운

- SW2의 `Gi1/0/3`: **Designated Port (DP)**
- SW3의 `Gi1/0/2`: **Blocked(BLK)** 상태
- 이미 차단 상태이므로 **트래픽 영향 없음**
- **SW2와 SW3는 루트 브리지(SW1)로 TCN 전송**
- 루트 브리지는 구성 BPDU에 **Topology Change 플래그**를 설정
- 전체 네트워크의 **MAC 주소 테이블 aging 타이머가 15초로 단축**

---

### 📘 직접 링크 장애 시나리오 2: SW1 ↔ SW3 링크 다운

- SW3의 `Gi1/0/2`: **Root Port (RP)** 상태 → 루트 브리지(SW1)와의 통신 단절
- **SW3는 TCN 전송 시도**, 하지만 **루트 포트가 없어 실패**
- 루트 브리지인 **SW1은 TCN을 전송하지 않음**
- SW2 ↔ SW3 링크는 살아 있으므로, **SW3는 해당 링크를 통해 루트에 접근 가능**
- → `Gi1/0/2`는 새로운 RP가 되고, **STP 포트 전이 상태**로 진입

---

### 🗂️ Figure 2-3 요약: 장애 발생 후 수렴 단계

1. **SW1**이 구성 BPDU를 전송
2. **SW1**은 루트 브리지이므로 TCN 전송 **불필요**
3. **SW3**는 TCN 전송 시도하지만 루트 포트 부재로 **실패**
4. **SW2와 SW3**는 **MAC 테이블 aging 타이머를 15초로 단축**
5. **SW3**는 `Listening → Learning` 상태로 포트 전이 수행

---

### ⏱️ 수렴 단계 요약 (시나리오 2 기준)

1. **SW1**이 링크 장애를 감지
2. **SW3**는 BPDU 수신 불가로 **TCN 전송 시도**
3. **SW1**은 구성 BPDU를 전파
4. **SW2, SW3**는 **MAC aging 타이머를 15초로 설정**
5. **SW3**는 `Listening` 상태 진입
6. 약 **30초 후**, `Gi1/0/2`가 **새로운 Root Port(RP)**로 설정됨

---

### 📘 직접 링크 장애 시나리오 3: SW1 ↔ SW2 링크 다운

- SW2는 더 이상 **BPDU를 수신할 수 없음**
- `Gi1/0/1` 포트가 다운되며 SW2는 **TCN 전송을 시도**
- 하지만 SW2는 루트 브리지가 아니므로 **구성 BPDU 전송 불가**
- 네트워크는 **MAC 테이블 aging**, 포트 전이 등 일반적인 수렴 절차 수행

---
## ⚡ Rapid Spanning Tree Protocol 요약 (RSTP)

### 🧩 배경 및 발전

- **802.1D STP**는 레이어 2 루프 방지에는 효과적이지만,
  - **수렴 속도와 확장성에 한계**가 있음
- Cisco는 이를 보완하기 위해 **PVST, PVST+**를 개발
- IEEE는 이를 표준화하여 **802.1w = RSTP**로 통합

---

### 🔁 RSTP (802.1w) 포트 상태

| 상태       | 설명 |
|------------|------|
| **Discarding** | 트래픽 전달 불가 (STP의 Blocking + Listening 통합) |
| **Learning**   | MAC 학습 가능, 트래픽 전달 불가 |
| **Forwarding** | MAC 학습 가능, 트래픽 전달 가능 |

> 🔸 **핸드셰이크 실패 시**, 포트는 자동으로 **STP(802.1D) 모드로 전환**

---

### 🧭 RSTP 포트 역할

| 역할            | 설명 |
|------------------|------|
| **Root Port (RP)**      | 루트 브리지로 향하는 최단 경로 포트 |
| **Designated Port (DP)**| 세그먼트당 유일하게 트래픽을 전달하는 포트 |
| **Alternate Port**      | RP 장애 발생 시 사용되는 **백업 경로** |
| **Backup Port**         | 동일 충돌 도메인에서의 **백업 포트** |
| **Edge Port**           | 종단 장치와 연결된 포트 (PortFast 활성화됨) |

---

### 🔌 RSTP 포트 유형

| 포트 유형          | 설명 |
|---------------------|------|
| **Edge Port**        | 호스트(종단 장치)와 연결, 루프 없음 |
| **Non-Edge Port**    | BPDU를 수신 중인 포트 |
| **Point-to-Point Port** | RSTP 스위치 간의 전이중 연결 포트 (핸드셰이크 사용 가능) |

---

### 🛠️ RSTP 토폴로지 구축 절차

1. 두 스위치 간 연결 시, **P2P 링크 여부 확인**
2. **핸드셰이크(BPDU 교환)** 수행하여 **Designated Port(DP)** 결정
3. **우선순위(Bridge Priority) 및 MAC 주소**를 기준으로 우선/열등 스위치 결정
4. 열등 스위치는 **Root Port(RP)** 지정, 나머지 포트를 **Discarding 상태**로 이동
5. 열등 스위치는 동기화 완료 후 BPDU로 통지
6. 지정된 포트를 **Forwarding 상태**로 전이
7. **하위 스위치에 대해 이 과정을 반복**

---

### ⏱️ RSTP 수렴 속도

- Hello 메시지를 **3회 연속 미수신 시 포트 정보 만료**
- **STP의 Max Age 20초** → **RSTP는 6초 내 감지**
- 포트 상태 전이:
  - **Discarding → Learning → Forwarding**  
  - 빠른 전환 가능 (핸드셰이크 기반)
- 핸드셰이크 실패 시: **STP(802.1D) 모드로 강제 전환**

---

## 📝 시험 준비 팁 (Exam Preparation Tasks)

- Chapter 30 "Final Preparation"의 연습 문제 활용
- **Pearson Test Prep** 소프트웨어의 시험 시뮬레이션 문제 추천

### ✅ Review All Key Topics

- 이 장의 주요 개념들은 **Key Topic 아이콘**으로 표시됨
- 아래 표 2-3(교재 기준)은 각 주제와 페이지 정보를 요약

> ✨ 실제 시험 대비 시에는 포트 역할/상태 비교표, 장애 시 수렴 흐름도, 핸드셰이크 기반 전이 조건 등을 중점적으로 암기할 것


## 📘 표 2-3: Chapter 2의 주요 핵심 주제 (Key Topics)

| Key Topic Element | Description                         | Page |
|-------------------|-------------------------------------|------|
| **List**          | 802.1D 포트 유형                    | 39   |
| **Section**       | STP 핵심 용어                        | 39   |
| **Section**       | 루트 브리지 선출                     | 41   |
| **Section**       | 루트 포트 찾기                       | 44   |
| **Section**       | STP 토폴로지 변경                   | 49   |
| **Section**       | Rapid Spanning Tree Protocol (RSTP) | 53   |
| **Section**       | RSTP (802.1W) 포트 상태              | 54   |
| **Section**       | RSTP 토폴로지 구성                   | 55   |

---

## 🧠 Complete Tables and Lists from Memory

> 이 장에는 **암기해야 할 테이블은 따로 없습니다.**  
> 그러나 위 **Key Topics**는 시험 대비 필수 영역으로 숙지할 것.

---

## 📝 Define Key Terms

아래 용어들은 이 장의 핵심 개념입니다.  
각 용어의 정의를 기억하고, 글로서리에서 정확한 의미를 복습하세요:

- **BPDU**
- **Configuration BPDU**
- **Designated Port (DP)**
- **Forward Delay**
- **Hello Time**
- **Local Bridge Identifier**
- **Max Age**
- **Root Port**
- **Root Bridge**
- **Root Path Cost**
- **Root System Priority**
- **System ID Extension**
- **Topology Change Notification (TCN)**

---

## 🧪 Use the Command Reference to Check Your Memory

다음 표는 이 장에서 다룬 **핵심 명령어**들을 요약한 것입니다.  
표의 **설명만 보고 명령어를 스스로 떠올려보며 암기 테스트**에 활용해보세요:

| 설명 | 명령어 |
|------|--------|
| VLAN별 STP 상태 전체 보기 | `show spanning-tree vlan [vlan-id]` |
| 특정 포트의 STP 상태 확인 | `show spanning-tree interface [interface-id]` |
| STP 루트 브리지 확인 | `show spanning-tree root` |
| 상세 Topology Change 이력 확인 | `show spanning-tree vlan [vlan-id] detail` |

---

> ✅ **Tip**: 명령어를 완전히 기억한 후, 실제 시뮬레이터 또는 장비에서 실습하며 정확한 출력 구조도 익혀두는 것이 좋습니다.

## 🧾 표 2-4: 명령어 참조표 (Command Reference)

### 🔧 STP 파라미터 설정 명령어

| Task 설명                        | 명령어 구문 |
|----------------------------------|-------------|
| STP 최대 수명(Max Age) 설정     | `spanning-tree vlan [vlan-id] max-age` |
| STP Hello 타이머 설정           | `spanning-tree vlan [vlan-id] hello-time` |
| STP 포워딩 지연(Forward Delay) 설정 | `spanning-tree vlan [vlan-id] forward-time` |

---

### 🔍 STP 정보 조회 명령어

| 작업 설명                                                   | 명령어 구문 |
|--------------------------------------------------------------|-------------|
| STP 루트 브리지 및 비용 표시                                 | `show spanning-tree root` |
| 하나 이상의 VLAN에 대한 STP 정보 표시 (루트, 로컬 브리지 등) | `show spanning-tree [vlan vlan-id]` |
| 마지막 TCN 발생 시점 및 관련 포트 식별                        | `show spanning-tree [vlan vlan-id] detail` |

---

> ✅ **Tip:** 모든 명령어는 전역 구성 모드(Global Configuration Mode) 또는 특권 EXEC 모드에서 실행합니다.  
> 시뮬레이터(Cisco Packet Tracer, CML 등) 또는 실제 장비에서 직접 실습하여 **출력 구조까지 함께 확인**해보세요.
