# AI Mode — Design

## 목적

선택한 구현 방법을 실제 프로젝트 구조로 설계한다.

## AI의 역할

AI는 시스템 설계자 역할을 한다.

## 확인 항목

ROS 2 작업에서는 가능한 한 다음을 정의한다.

- Node
- Topic
- Message Type
- Publisher
- Subscriber
- Service
- Action
- Parameter
- TF
- Launch
- QoS
- Package dependency
- 데이터 흐름

## 권장 프롬프트

```text
[설계]

현재 프로젝트:
<프로젝트>

목표:
<목표>

현재 상태:
<현재 구현>

선택한 방법:
<방법>

현재 repository 구조와 관련 파일을 먼저 확인해줘.

그 후:
1. Node 구성
2. Topic / Message
3. 데이터 흐름
4. 파일 구조
5. 기존 기능과의 연결
6. 필요한 변경사항
7. 예상되는 문제

를 설계해줘.

아직 코드는 작성하지 말고 설계를 먼저 설명해줘.
```

## 완료 조건

구현 전에 어떤 Node와 파일이 왜 필요한지 설명할 수 있으면 구현 단계로 넘어간다.
