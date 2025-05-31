# CHAPTER 2
## Spanning Tree Protocol

이 장에서는 다음과 같은 주제를 다룹니다:

- **Spanning Tree Protocol Fundamentals**: 이 섹션은 스위치가 다른 스위치를 인식하고 전달 루프를 방지하는 방법에 대한 개요를 제공합니다.
- **Rapid Spanning Tree Protocol**: 이 섹션은 더 빠른 수렴을 위해 STP에 적용된 개선 사항을 살펴봅니다.

좋은 네트워크 설계는 장치 및 네트워크 링크(즉, 경로)의 중복성을 제공합니다. 가장 간단한 해결책은 네트워크 링크 장애를 극복하거나 스위치가 토폴로지 내에서 최소 두 개 이상의 다른 스위치에 연결되어 있도록 하는 것입니다. 그러나 이러한 토폴로지는 브로드캐스트 또는 알려지지 않은 유니캐스트 플러딩(unknown unicast flooding) 트래픽을 스위치가 전달해야 할 경우 문제가 발생할 수 있습니다. 네트워크 브로드캐스트는 루프를 통해 연속적으로 전달되어 링크가 포화되고 스위치는 패킷을 드롭하게 됩니다. 또한 MAC 주소 테이블은 패킷이 루프를 도는 동안 지속적으로 변경되어 토폴로지가 안정되지 않게 됩니다. 이러한 루프는 Layer 2 포워딩에 TTL(Time to Live) 메커니즘이 없기 때문에 발생합니다. 스위치의 CPU 사용률이 증가하고, 메모리 소비도 증가하여 결국 스위치가 다운될 수 있습니다.

이 장에서는 스위치가 어떻게 전달 루프를 방지하면서도 중복 링크를 허용하는지를 Spanning Tree Protocol(STP) 및 Rapid Spanning Tree Protocol(RSTP)을 사용하여 설명합니다.
이외에도 두 개의 추가 장이 STP 관련 주제를 설명합니다:

- Chapter 3, "Advanced STP Tuning": BPDU guard 및 BPDU 필터와 같은 고급 STP 주제를 다룹니다.
- Chapter 4, "Multiple Spanning Tree Protocol": MST(Multiple Spanning Tree Protocol)을 다룹니다.

---

## "Do I Know This Already?" Quiz

> "Do I Know This Already?" 퀴즈는 독자가 해당 장 전체를 읽을 필요가 있는지 여부를 판단할 수 있도록 도와줍니다. 이 셀프 테스트 질문 중 하나 이상을 틀리면, "Exam Preparation Tasks" 섹션으로 넘어가기 전에 이 장 전체를 읽는 것이 좋습니다.
> 표 2-1은 이 장의 주요 제목 아래에 해당하는 퀴즈 문항을 정리한 것입니다. Appendix A에 이 퀴즈의 해답이 나와 있습니다.

### Table 2-1 "Do I Know This Already?" Foundation Topics Section-to-Question Mapping

| Foundation Topics Section           | Questions |
|-------------------------------------|-----------|
| Spanning Tree Protocol Fundamentals | 1–6       |
| Rapid Spanning Tree Protocol        | 7–9       |

---

### 1. BPDU 유형은 총 몇 가지가 있나요?
- a. 하나
- b. 두 개
- c. 세 개
- d. 네 개

**정답:** b. 두 개  
**해설:** Configuration BPDU와 Topology Change Notification (TCN) BPDU 두 가지가 있습니다【이미지 p.40 참조】.

---

### 2. 루트 브리지를 선출하는 데 사용되는 주요 속성은 무엇인가요?
- a. 스위치 포트 우선순위
- b. 브리지 우선순위
- c. 스위치 일련번호
- d. 경로 비용

**정답:** b. 브리지 우선순위  
**해설:** 브리지 우선순위가 낮을수록 루트 브리지로 선택될 가능성이 높습니다. (동일한 경우 MAC 주소 기준)【p.42】.

---

### 3. 원래 802.1D 명세는 1Gbps 인터페이스에 어떤 포트 비용 값을 할당하나요?
- a. 1
- b. 2
- c. 4
- d. 19

**정답:** c. 4  
**해설:** 표 2-2 참조. 1Gbps는 Short-Mode 기준 포트 비용 4【p.41】.

---

### 4. 루트 브리지에 있는 모든 포트의 역할은 무엇인가요?
- a. 루트 포트
- b. 지정 포트
- c. 상위 포트
- d. 마스터 포트

**정답:** b. 지정 포트  
**해설:** 루트 브리지는 전송을 위한 모든 포트가 지정 포트로 설정됩니다【p.39】.

---

### 5. 기본 설정에서, 포트가 Listening 상태에 머무는 시간은 얼마인가요?
- a. 2초
- b. 5초
- c. 10초
- d. 15초

**정답:** b. 15초  
**해설:** Forward delay 타이머는 기본적으로 15초입니다【p.40】.

---

### 6. 토폴로지 변경 플래그가 설정된 설정 BPDU를 수신할 때, 하위 스위치는 어떻게 반응하나요?
- a. 모든 포트를 차단 상태로 이동시킨다.
- b. MAC 주소 테이블에서 모든 MAC 주소를 제거한다.
- c. 모든 비루트 포트를 일시적으로 Listening 상태로 이동시킨다.
- d. MAC 주소 테이블에서 오래된 MAC 주소만 제거한다.
- e. 로컬 스위치 데이터베이스에 토폴로지 변경 플래그 버전을 업데이트한다.

**정답:** b. MAC 주소 테이블에서 모든 MAC 주소를 제거한다.  
**해설:** 토폴로지 변경시 모든 MAC 주소를 제거하여 다시 학습합니다【p.40】.

---

### 7. 다음 중 RSTP 포트 상태가 아닌 것은 무엇인가요?
- a. Blocking
- b. Listening
- c. Learning
- d. Forwarding

**정답:** b. Listening  
**해설:** RSTP에서는 Listening 상태가 존재하지 않습니다【p.38】.

---

### 8. 참/거짓: 802.1D와 비교했을 때 RSTP는 포트 상태 수를 5개에서 4개로 줄인다.
- a. 참
- b. 거짓

**정답:** a. 참  
**해설:** Disabled, Blocking, Listening, Learning, Forwarding → RSTP는 이를 간소화합니다【p.38】.

---

### 9. 참/거짓: RSTP를 실행 중인 대규모 L2 토폴로지에서는, 패킷이 전달되기 전에 반드시 전체 인프라가 완전히 수렴해야 한다.
- a. 참
- b. 거짓

**정답:** b. 거짓  
**해설:** RSTP는 빠른 수렴을 지원하며, 전체 인프라가 수렴될 때까지 기다릴 필요는 없습니다【p.38】.

---

## Foundation Topics

### Spanning Tree Protocol Fundamentals

**Spanning Tree Protocol(STP)**은 스위치가 다른 스위치에 대한 인식을 가능하게 하여 전달 루프를 방지할 수 있도록 합니다. 이는 **브리지 프로토콜 데이터 유닛(BPDU)**의 전송 및 수신을 통해 이루어집니다. STP는 중복 포트를 임시로 차단하여 루프 없는 레이어 2 토폴로지를 구축합니다. 이는 최적의 전송 경로를 선택하고 트리 기반 알고리즘을 실행하여 중복 포트가 트래픽을 전달하지 않도록 합니다.

STP에는 여러 가지 구현 버전이 있습니다:
- 802.1D: STP의 원래 명세
- Per-VLAN Spanning Tree (PVST)
- Per-VLAN Spanning Tree Plus (PVST+)
- 802.1w Rapid Spanning Tree Protocol (RSTP)
- 802.1s Multiple Spanning Tree Protocol (MST)

Catalyst 스위치는 PVST+, RSTP, MST 모드로 작동하며, 이 세 가지 모드는 모두 802.1D와 호환됩니다.

---

### IEEE 802.1D STP

STP의 원래 버전은 IEEE 802.1D 표준에서 유래되었으며, 하나의 VLAN에 대해 루프 없는 토폴로지를 보장합니다. 이 주제는 Rapid Spanning Tree Protocol(RSTP)과 Multiple Spanning Tree Protocol(MST)의 기반이 되므로 매우 중요합니다.

#### 802.1D 포트 상태

- **Disabled**: 포트가 관리적으로 비활성화된 상태입니다 (예: shutdown).
- **Blocking**: 스위치 포트는 활성화되어 있지만, 트래픽을 전달하지 않으며 루프 방지를 위해 사용됩니다. 이 상태에서 MAC 주소 테이블은 수정되지 않고, BPDU 수신만 가능합니다.
- **Listening**: 포트가 Blocking에서 전이되어 BPDU 수신은 가능하지만 트래픽 전달은 불가능합니다. 이 상태의 지속 시간은 STP의 전이 시간(포워딩 시간)에 따라 결정됩니다.
- **Learning**: 포트가 수신한 트래픽을 기반으로 MAC 주소 테이블을 수정할 수 있지만, 여전히 트래픽 전달은 하지 않습니다.
- **Forwarding**: 포트가 전체 트래픽을 전달하며 MAC 주소 테이블도 업데이트합니다. 이는 최종 포워딩 상태입니다.
- **Broken**: 구성 오류나 운영상의 문제로 인해 발생하며, 문제가 해결될 때까지 패킷을 폐기합니다.

> 참고: 전체 STP 초기화에는 약 30초가 걸리며, 기본 타이머를 사용할 경우 Blocking에서 Forwarding으로 전이하는 데 필요한 시간입니다.

#### 802.1D 포트 유형

- **Root Port (RP)**: 루트 브리지 또는 상위 스위치에 연결되는 네트워크 포트입니다. VLAN당 하나의 루트 포트만 존재해야 합니다.
- **Designated Port (DP)**: 다른 스위치에 BPDU 프레임을 전달하는 포트입니다. 링크당 단 하나의 활성 지정 포트만 있어야 합니다.
- **Blocking Port**: STP 계산 결과에 따라 트래픽을 전달하지 않는 포트입니다.

---

### STP 주요 용어

- **Root Bridge**: Layer 2 토폴로지에서 가장 중요한 스위치입니다. 모든 포트가 포워딩 상태입니다. 이 스위치는 스패닝 트리 계산의 기준점이며, 다른 모든 스위치는 루트 브리지로부터의 경로 비용을 기준으로 계산을 수행합니다. 루트 브리지의 모든 포트는 **지정 포트(Designated Port)**로 분류됩니다.

---

### Bridge Protocol Data Unit (BPDU)

BPDU는 네트워크 스위칭 환경에서 계층 구조를 식별하고 토폴로지 변경 사항을 알리는 데 사용되는 네트워크 패킷입니다. BPDU는 MAC 주소 `01:80:c2:00:00:00`을 사용하며, 두 가지 유형이 있습니다:

- **Configuration BPDU**: 이 BPDU는 루트 브리지, 루트 포트, 지정 포트 및 차단 포트를 식별하는 데 사용됩니다. 다음과 같은 필드를 포함합니다:
    - STP 타입
    - 루트 경로 비용
    - 루트 브리지 식별자
    - 로컬 브리지 식별자
    - Max Age
    - Hello Time
    - Forward Delay
- **Topology Change Notification (TCN) BPDU**: 이 BPDU는 Layer 2 토폴로지 변경 사항을 다른 스위치에 알리기 위해 사용됩니다.

---

### Root Path Cost

Root Path Cost는 루트 스위치까지 특정 경로에 대한 누적 비용입니다.

- **System Priority**: 루트 브리지를 선출할 때 사용되는 4비트 우선순위. 기본값은 32,768입니다.
- **System ID Extension**: VLAN ID를 나타내는 12비트 값으로, 브리지 ID에 포함됩니다.
- **Local Bridge Identifier**: MAC 주소 + System Priority + System ID Extension의 조합.
- **Max Age**: 브리지가 BPDU 정보를 저장하는 최대 시간(기본값 20초).
- **Hello Time**: 포트에서 BPDU를 전송하는 주기 (기본값 2초).
- **Forward Delay**: 포트가 Listening 및 Learning 상태에 머무는 시간. 기본값은 15초이며, 총 30초가 필요합니다.

> 참고: STP는 스위치보다 먼저 존재한 프로토콜입니다. 초기에는 이 기술을 사용하는 장비들이 "브리지"로 불렸고, 현재의 스위치와 같은 역할을 했습니다. 이 문맥에서 bridge와 switch는 동일하게 사용됩니다.

---

### Spanning Tree Path Cost

STP 경로 비용은 루트 브리지까지의 누적 STP 인터페이스 비용입니다. 이 비용은 다음과 같은 기준으로 측정됩니다:

- 원래는 16비트 값으로 저장되며 기준 대역폭은 20Gbps였습니다.
- 최신 방식인 long mode는 32비트 값을 사용하며 기준 대역폭은 20Tbps입니다.
- 대부분의 스위치에서는 이제 기본값이 short mode이며, 필요에 따라 long mode로 변경할 수 있습니다.

#### 표 2-2: STP 포트 비용 (Short / Long 모드)

| 링크 속도   | Short-Mode STP 비용 | Long-Mode STP 비용 |
|-------------|---------------------|-------------------|
| 10 Mbps     | 100                 | 2,000,000         |
| 100 Mbps    | 19                  | 200,000           |
| 1 Gbps      | 4                   | 20,000            |
| 10 Gbps     | 2                   | 2,000             |
| 20 Gbps     | 1                   | 1,000             |
| 100 Gbps    | 1                   | 200               |
| 1 Tbps      | 1                   | 20                |
| 10 Tbps     | 1                   | 2                 |

---

### STP 토폴로지 구축

STP의 첫 번째 단계는 **루트 브리지(Root Bridge)**를 식별하는 것입니다. 각 스위치는 처음에는 자신이 루트 브리지라고 가정하고 자신의 로컬 브리지 ID를 사용하여 구성 BPDU를 전송합니다. 이후 인접한 스위치의 구성 BPDU를 수신하면서 다음과 같은 판단을 합니다:

- 인접 스위치의 BPDU가 더 나쁘면, 자신의 BPDU를 유지
- 인접 스위치의 BPDU가 더 낫다면, 자신의 BPDU를 업데이트하고 새로운 루트 브리지 ID 및 누적 루트 경로 비용을 반영하여 BPDU를 다시 전송

이 과정은 토폴로지 내의 모든 스위치가 루트 브리지를 인식할 때까지 반복됩니다.

---

#### [Figure 2-1] 기본 STP 토폴로지

STP는 브리지 식별자의 우선순위가 낮을수록 선호합니다. BPDU에 있는 우선순위가 더 낮으면 해당 스위치가 더 선호되며 루트 브리지로 선정됩니다. 만약 우선순위가 같다면, MAC 주소가 더 낮은 스위치가 루트 브리지가 됩니다.

일반적으로 오래된 스위치는 MAC 주소가 낮기 때문에 루트 브리지로 더 선호됩니다.
구성 변경이 없다면, 기존 스위치가 루트 브리지를 유지하고, 새로운 스위치가 연결되더라도 루트 브리지로 승격되지 않도록 보장할 수 있습니다.

예시:
- SW1이 루트 브리지로 식별됨
- MAC 주소 0062.ec9d.c500이 네트워크 내에서 가장 낮음
- `show spanning-tree root` 명령을 사용하여 SW1의 루트 브리지 상태를 확인 가능

---

#### Example 2-1: STP 루트 브리지 확인

```shell
SW1# show spanning-tree root

VLAN     Root ID               Cost  Hello Max Fwd  Root Port
-------- -------------------- ----- ------ -------- ----------
VLAN0001 32769 0062.ec9d.c500   1      2     20    15   —
VLAN0010 32778 0062.ec9d.c500   1      2     20    15   —
VLAN0020 32788 0062.ec9d.c500   1      2     20    15   —
VLAN0099 32867 0062.ec9d.c500   1      2     20    15   —
```

루트 포트가 표시되지 않는 것은 SW1이 루트 브리지이기 때문입니다. 모든 포트는 지정 포트로 설정됩니다.

---

(이하 전체 텍스트에 대해 동일한 마크다운 서식 적용)
