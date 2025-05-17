# Chapter 3. Advanced STP Tuning

## 1. STP 최적화 필요성
- 네트워크 복원 시간 단축, 트래픽 경로 최적화

## 2. 주요 튜닝 항목
- **Root Bridge 수동 지정**: 중요 스위치가 Root가 되도록 priority 조정
- **PortFast**: 엣지 포트에서 STP 대기 없이 바로 Forwarding (PC, 프린터 등)
- **BPDU Guard**: PortFast 포트에서 BPDU 수신 시 포트 비활성화
- **Root Guard**: 비정상적인 Root Bridge 변경 방지
- **UplinkFast/BackboneFast**: 장애 복구 시간 단축

## 3. 명령어 예시
- `spanning-tree vlan <번호> priority <값>`
- `spanning-tree portfast`
- `spanning-tree bpduguard enable`
- `spanning-tree guard root`
