# Chapter 4. Multiple Spanning Tree Protocol 실습

## [실습1] MSTP 활성화 및 인스턴스 설정

1. MST 모드로 전환
   ```shell
   spanning-tree mode mst
   ```
2. MST 인스턴스와 VLAN 매핑 설정
   ```shell
   spanning-tree mst configuration
   instance 1 vlan 10,20
   instance 2 vlan 30,40
   ```
3. 설정 확인
   ```shell
   show spanning-tree mst
   ```

---

## [실습2] MST Region 구성

1. 여러 스위치에 동일한 MST 이름, revision, VLAN 매핑 적용
2. Region이 정상적으로 형성되는지 확인
