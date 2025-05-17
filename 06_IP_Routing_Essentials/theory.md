# Chapter 6. IP Routing Essentials

## 1. 라우팅이란?
- 서로 다른 네트워크 간에 패킷을 전달하는 과정
- 라우터/Layer3 스위치가 수행

## 2. 라우팅 프로토콜 분류
- **정적 라우팅**: 수동으로 경로 지정
- **동적 라우팅**: 라우터끼리 경로 정보를 자동 교환 (RIP, OSPF, EIGRP 등)

## 3. 라우팅 테이블
- 목적지 네트워크, 넥스트 홉, 인터페이스 정보 포함

## 4. 주요 명령어
- `show ip route`
- `ip route <네트워크> <마스크> <넥스트홉>`
- `show running-config`
