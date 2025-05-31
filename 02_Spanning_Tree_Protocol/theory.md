# CHAPTER 2  
## Spanning Tree Protocol

---

### 이 장에서는 다음과 같은 주제를 다룹니다:
- **Spanning Tree Protocol Fundamentals**: 스위치가 다른 스위치를 인식하고 전달 루프를 방지하는 방법 개요  
- **Rapid Spanning Tree Protocol**: 더 빠른 수렴을 위한 STP 개선 사항

---

## 네트워크 설계와 루프 문제

좋은 네트워크 설계는 장치 및 링크의 **중복성**을 고려합니다. 하지만 레이어 2에서는 루프가 발생할 수 있으며, 이는 브로드캐스트 폭주, MAC 주소 테이블 불안정, CPU 과부하를 초래합니다.  
이를 해결하기 위해 **Spanning Tree Protocol (STP)**와 **Rapid Spanning Tree Protocol (RSTP)**을 사용합니다.

> 참고: STP는 포워딩 루프를 방지하며 중복 링크를 차단 포트로 설정하여 네트워크 안정성을 확보합니다.

---

## 퀴즈: "Do I Know This Already?"

| Section | Questions |
|---------|-----------|
| STP Fundamentals | 1–6 |
| RSTP | 7–9 |

### 1. BPDU 유형 수  
**정답: b. 두 개**  
**해설:** Configuration BPDU와 Topology Change Notification (TCN) BPDU

### 2. 루트 브리지 선출 기준  
**정답: b. 브리지 우선순위**  
**해설:** 낮은 브리지 우선순위 → 루트 브리지로 선택

### 3. 1Gbps의 포트 비용 (802.1D)  
**정답: c. 4**

### 4. 루트 브리지의 모든 포트 역할  
**정답: b. 지정 포트 (Designated Port)**

### 5. Listening 상태 유지 시간  
**정답: d. 15초**

### 6. 토폴로지 변경 BPDU 수신 시 반응  
**정답: b. MAC 테이블 초기화**

### 7. RSTP에 존재하지 않는 포트 상태  
**정답: b. Listening**

### 8. RSTP 포트 상태 수  
**정답: a. 참** — STP 5개 → RSTP 3개

### 9. RSTP 수렴 조건  
**정답: b. 거짓** — 전체 인프라 수렴 전에도 패킷 전달 가능

---

## STP Fundamentals

### STP 주요 역할
- 루프 방지
- 최적 경로 선택
- 중복 포트 차단

### STP 구현 종류
- 802.1D
- PVST / PVST+
- RSTP (802.1w)
- MST (802.1s)

---

## IEEE 802.1D STP 구조

### 포트 상태
1. Disabled  
2. Blocking  
3. Listening  
4. Learning  
5. Forwarding  
6. Broken (Cisco 확장)

### 포트 유형
- **Root Port (RP)**: 루트 브리지로 가는 포트  
- **Designated Port (DP)**: 세그먼트당 하나  
- **Blocking Port**: 루프 방지용 비활성 포트

---

## 핵심 용어 정의

| 용어 | 설명 |
|------|------|
| **BPDU** | 브리지 간 STP 메시지 |
| **Root Bridge** | 최상위 브리지 (모든 포트가 DP) |
| **Root Path Cost** | 루트까지 누적 경로 비용 |
| **System Priority** | 루트 선출 시 우선순위 (기본값 32768) |
| **System ID Extension** | VLAN ID를 포함하는 필드 |
| **Forward Delay** | Listening + Learning 시간 합 (기본 30초) |

---

## 경로 비용 표 (Short / Long 모드)

| 링크 속도 | Short | Long |
|-----------|-------|------|
| 10 Mbps   | 100   | 2,000,000 |
| 100 Mbps  | 19    | 200,000   |
| 1 Gbps    | 4     | 20,000    |
| 10 Gbps   | 2     | 2,000     |

---

## 루트 브리지 선출 및 확인

```bash
SW1# show spanning-tree root
```

---

## 지정 포트 차단 기준

1. 루트 포트 제외
2. 경로 비용 높은 쪽 차단
3. 시스템 우선순위 높은 쪽
4. MAC 주소 높은 쪽

---

## RSTP (802.1w)

### 포트 상태
- **Discarding**
- **Learning**
- **Forwarding**

### 포트 역할
- **Root Port**
- **Designated Port**
- **Alternate Port**
- **Backup Port**
- **Edge Port**

---

## RSTP 특징 요약

- 빠른 수렴 (6초 이내)
- 포트 상태 단순화
- 포트패스트 기본 적용
- STP와 호환 가능

---

## 명령어 요약

| 작업 | 명령어 |
|------|--------|
| Max Age 설정 | `spanning-tree vlan <vlan-id> max-age` |
| Hello 설정 | `spanning-tree vlan <vlan-id> hello-time` |
| Forward Delay 설정 | `spanning-tree vlan <vlan-id> forward-time` |
| 루트 브리지 확인 | `show spanning-tree root` |
| STP 상태 확인 | `show spanning-tree vlan <vlan-id>` |
| 상세 변경 사항 확인 | `show spanning-tree vlan <vlan-id> detail` |

---

## 용어 정리

BPDU, Configuration BPDU, Designated Port (DP), Forward Delay, Hello Time, Local Bridge Identifier, Max Age, Root Port, Root Bridge, Root Path Cost, Root System Priority, System ID Extension, TCN**

---

이 파일은 CCNP ENCOR 시험 대비를 위한 스패닝 트리 프로토콜의 핵심 내용을 정리한 것입니다.
