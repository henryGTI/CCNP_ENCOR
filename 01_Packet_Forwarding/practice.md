# Chapter 1. Packet Forwarding 실습

## [실습1] 스위치 MAC 주소 테이블 확인

### 1. 실습 토폴로지
- 스위치 1대, PC 2대 (PC1, PC2)
- PC1과 PC2를 스위치에 각각 연결

### 2. 실습 절차
1. PC1에서 PC2로 ping 실행
2. 스위치에서 다음 명령어 입력
   ```shell
   show mac address-table
   ```
3. 각 포트에 어떤 MAC이 학습됐는지 확인

### 3. 결과 해설
- ping이 오가면 스위치가 각 포트에 연결된 PC의 MAC 주소를 학습
- MAC 주소 테이블에 PC1, PC2의 MAC과 연결 포트가 표시됨

---

## [실습2] 라우터의 라우팅 테이블 확인

### 1. 실습 토폴로지
- 라우터 1대, PC 2대 (서로 다른 네트워크에 위치)
- 예시: PC1(192.168.1.2/24), PC2(192.168.2.2/24), 라우터에 각각 연결

### 2. 실습 절차
1. 라우터에서 다음 명령어 입력
   ```shell
   show ip route
   ```
2. 라우팅 테이블에서 각 네트워크로 가는 경로 확인

### 3. 결과 해설
- 라우터는 직접 연결된 네트워크와 정적/동적 경로를 테이블에 저장
- 목적지 IP에 따라 올바른 인터페이스로 패킷을 포워딩

---

## [실습3] VLAN 환경에서 포워딩

### 1. 실습 토폴로지
- 스위치 1대, PC 2대 (각각 VLAN 10, VLAN 20에 연결)

### 2. 실습 절차
1. 스위치에서 VLAN 10, 20 생성
   ```shell
   vlan 10
   vlan 20
   ```
2. 각 포트에 VLAN 할당
   ```shell
   interface range fa0/1 - 2
   switchport mode access
   switchport access vlan 10
   interface range fa0/3 - 4
   switchport mode access
   switchport access vlan 20
   ```
3. 같은 VLAN끼리만 ping이 되는지 확인

### 3. 결과 해설
- 같은 VLAN끼리만 통신 가능, 다른 VLAN은 라우터/Layer3 스위치 필요

---

## 실습 팁

- 실습은 GNS3, Cisco Packet Tracer 등에서 진행하면 좋습니다.
- 각 명령어의 출력 결과를 캡처해 정리하면 학습에 도움이 됩니다.
