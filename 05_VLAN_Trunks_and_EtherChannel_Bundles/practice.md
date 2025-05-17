# Chapter 5. VLAN Trunks and EtherChannel Bundles 실습

## [실습1] 트렁크 포트 설정

1. 두 스위치 연결 포트에 트렁크 설정
   ```shell
   interface fa0/24
   switchport mode trunk
   switchport trunk allowed vlan 10,20
   ```

---

## [실습2] EtherChannel 구성

1. 여러 포트를 EtherChannel로 묶기
   ```shell
   interface range fa0/1 - 2
   channel-group 1 mode active
   ```
2. EtherChannel 상태 확인
   ```shell
   show etherchannel summary
   ```

---

## [실습3] DTP 동작 확인

1. 포트에 DTP 모드 설정
   ```shell
   switchport mode dynamic desirable
   ```
2. 트렁크 협상 결과 확인
