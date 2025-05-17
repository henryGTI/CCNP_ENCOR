# Chapter 2. Spanning Tree Protocol 실습

## [실습1] Root Bridge 확인

1. 스위치 3대 연결, 각 스위치에 우선순위(Bridge Priority) 다르게 설정
2. 각 스위치에서 다음 명령어 실행
   ```shell
   show spanning-tree
   ```
3. Root Bridge가 어느 스위치인지 확인

---

## [실습2] 포트 상태 확인

1. STP가 활성화된 상태에서 각 포트의 상태 확인
   ```shell
   show spanning-tree
   ```
2. 어떤 포트가 Blocking, Forwarding 상태인지 확인

---

## [실습3] 루프 환경에서 STP 동작 확인

1. 스위치 2대를 루프가 생기도록 연결
2. STP가 정상적으로 루프를 차단하는지 확인
