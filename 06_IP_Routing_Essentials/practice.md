# Chapter 6. IP Routing Essentials 실습

## [실습1] 정적 라우팅 설정

1. 라우터에서 정적 경로 추가
   ```shell
   ip route 192.168.2.0 255.255.255.0 10.0.0.2
   ```
2. 라우팅 테이블 확인
   ```shell
   show ip route
   ```

---

## [실습2] 동적 라우팅 프로토콜 활성화 (예: RIP)

1. RIP 활성화 및 네트워크 등록
   ```shell
   router rip
   version 2
   network 192.168.1.0
   network 192.168.2.0
   ```
2. 라우팅 테이블에 RIP 경로가 추가되는지 확인

---

## [실습3] 경로 우선순위(Administrative Distance) 실습

1. 정적/동적 경로가 동시에 존재할 때 어떤 경로가 우선되는지 확인
