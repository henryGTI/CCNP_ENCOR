# Chapter 3. Advanced STP Tuning 실습

## [실습1] Root Bridge 수동 지정

1. 스위치에서 Root Bridge로 만들고 싶은 장비에 우선순위 낮게 설정
   ```shell
   spanning-tree vlan 1 priority 4096
   ```
2. Root Bridge가 변경되는지 확인

---

## [실습2] PortFast와 BPDU Guard 설정

1. PC가 연결된 포트에 PortFast 적용
   ```shell
   interface fa0/1
   spanning-tree portfast
   spanning-tree bpduguard enable
   ```
2. 해당 포트에 스위치 연결 시 포트가 shutdown 되는지 확인

---

## [실습3] Root Guard 실습

1. 특정 포트에 Root Guard 적용
   ```shell
   interface fa0/2
   spanning-tree guard root
   ```
2. 비정상적인 Root Bridge 변경 시 포트가 어떻게 동작하는지 확인
