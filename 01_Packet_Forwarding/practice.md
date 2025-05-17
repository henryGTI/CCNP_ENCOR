# Chapter 1. Packet Forwarding 실습

## [실습1] 스위치 MAC 주소 테이블 확인

1. 스위치와 PC 2대 연결
2. PC1 → PC2로 ping
3. 스위치에서 다음 명령어 실행:
   ```shell
   show mac address-table
   ```
   → 각 포트에 어떤 MAC이 학습됐는지 확인

---

## [실습2] 라우터의 라우팅 테이블 확인

1. 라우터와 PC 2대, 각각 다른 네트워크에 연결
2. 라우터에서 다음 명령어 실행:
   ```shell
   show ip route
   ```
   → 목적지 네트워크로 가는 경로 확인

---

## [실습3] VLAN 환경에서 포워딩

1. 스위치에 VLAN 10, 20 생성
2. 각 VLAN에 PC 연결
3. 같은 VLAN끼리만 통신되는지 ping으로 확인

---

## 실습 팁

- 실습은 GNS3, Cisco Packet Tracer 등에서 진행하면 좋습니다.
- 각 명령어의 출력 결과를 캡처해 정리하면 학습에 도움이
