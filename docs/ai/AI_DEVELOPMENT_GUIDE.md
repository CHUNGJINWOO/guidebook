# AI Development Guide

> AI-Hub · LIMO · ROS 2 프로젝트 공통 AI 개발 협업 지침

## 1. 목적

AI를 단순한 질문·답변 또는 코드 생성 도구가 아니라 프로젝트 개발 과정의 협업 도구로 사용하기 위한 공통 지침이다.

기본 개발 루프:

```text
학습 → 조사 → 계획/설계 → 구현 → 빌드 → 실행 → 검증 → 디버깅 → 문서화 → 다음 작업
```

AI의 답변을 그대로 신뢰하거나 복사하지 않고 실제 프로젝트와 실행 결과를 기준으로 검증한다.

## 2. 핵심 원칙

### 2.1 목적과 구현 방법을 구분한다

사용자는 목표와 제약조건을 제시한다. AI는 가능한 구현 방법을 조사하고 제안한다.

> 목적과 제약은 사람이 정하고, 구현 방법은 AI와 함께 결정한다.

사용자가 구현 방법을 모르는 경우 억지로 구체적인 구현 방법을 프롬프트에 넣지 않는다.

### 2.2 현재 프로젝트 상태를 먼저 확인한다

코드 작업 전 가능한 경우 다음을 확인한다.

- repository 구조
- 관련 파일
- `package.xml`
- `setup.py` / `setup.cfg`
- launch / parameter 파일
- 기존 Node
- Topic / Service / Action
- Message Type
- TF
- 최근 빌드·실행 결과
- 오류 로그

확인하지 않은 파일, Topic, Package, TF frame, Parameter를 사실처럼 만들지 않는다.

### 2.3 사실과 추론을 구분한다

다음을 구분한다.

- **확인된 사실:** 실제 코드·파일·실행 결과에서 확인
- **공식 자료:** 교수님 자료 또는 공식 문서에서 확인
- **분석/추론:** 확인된 정보를 바탕으로 한 AI의 분석
- **제안:** AI가 권장하는 구현 방법

확인되지 않은 내용을 사실처럼 단정하지 않는다.

### 2.4 작은 단위로 구현한다

```text
최소 기능 → 빌드 → 실행 → 검증 → 다음 기능
```

기존 구조를 불필요하게 크게 변경하지 않는다.

## 3. 프로젝트 역할 구분

### AI-Hub
- 프로젝트 문서/코드 검색
- MCP 기반 Knowledge Hub
- 여러 AI/Agent가 프로젝트 지식을 활용할 수 있는 공통 인터페이스

### ros2-limo
- LIMO 로봇 구현
- 자율주행
- SLAM / Localization
- Nav2
- 장애물 회피
- 자율 순찰
- 안내 기능

### ros2
- ROS 2 개발환경
- WSL2 / Ubuntu
- 네트워크
- 원격 개발
- 서버 및 운영 인프라

프로젝트 간 역할을 혼동하지 않는다.

## 4. LIMO 자료 우선순위

1. 교수님 제공 `[WeGo] Limo_ROS2_교재.pdf`
2. AgileX / WeGo 공식 LIMO 자료
3. ROS 2 공식 문서
4. Navigation2 공식 문서
5. SLAM Toolbox 공식 문서
6. 커뮤니티 자료

자료 간 차이가 있으면 명시한다.

## 5. 학습 중심 협업

사용자는 프로젝트를 직접 구현하면서 학습한다. 따라서 코드만 제공하지 않는다.

가능한 경우 다음을 설명한다.

1. 무엇을 하는가?
2. 왜 필요한가?
3. 입력은 무엇인가?
4. 출력은 무엇인가?
5. ROS 2에서 어떤 역할을 하는가?
6. 다른 구현 방법은 무엇인가?
7. 현재 프로젝트에서는 왜 이 방법을 사용하는가?
8. 어떻게 검증하는가?

## 6. 코드 구현 원칙

구현 전:
- 현재 구조 확인
- 수정 파일 확인
- 변경 이유 설명
- 기존 기능과의 관계 확인

구현 후:
- Build
- Run
- Test
- 결과 확인

가능하면 전체 파일을 무조건 덮어쓰기보다 변경 부분을 명확히 한다.

## 7. ROS 2 작업 원칙

가능한 한 다음을 명확히 한다.

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

Topic은 다음 흐름으로 설명한다.

```text
Publisher
   ↓
Topic
   ↓
Message Type
   ↓
Subscriber
```

실제 존재 여부를 확인하지 않은 Topic 이름을 임의로 사용하지 않는다.

## 8. Nav2 작업 원칙

다음을 구분한다.

- Localization
- Global Planner
- Local Controller
- Costmap
- Behavior Tree
- Action interface
- `/cmd_vel`

직접 `/cmd_vel`을 제어하는 테스트 코드가 최종 Navigation 구조와 독립적인 실험용 코드인지 명확히 한다. 여러 Node의 `/cmd_vel` 충돌 가능성도 확인한다.

## 9. 테스트 원칙

코드 작성만으로 완료라고 판단하지 않는다.

```bash
colcon build --packages-select <package>
source install/setup.bash
ros2 node list
ros2 topic list
ros2 topic info <topic>
ros2 topic echo <topic>
ros2 interface show <interface>
```

필요하면 RViz2 또는 rqt를 활용한다.

## 10. 오류 분석 원칙

### 문제
실제로 발생한 오류와 증상

### 원인
로그와 코드에서 확인되는 원인 또는 가능한 원인

### 확인 방법
원인을 확인하기 위한 명령

### 해결
필요한 수정 또는 조치

### 결과
수정 후 변화

### 검증
정상 동작 확인 방법

원인이 확실하지 않다면 추측임을 명시한다.

## 11. 프로젝트 기록

다음을 기록한다.

- 발생한 오류
- 원인
- 실패한 접근
- 해결 방법
- 변경한 Parameter
- 테스트 결과
- 선택한 구현 방법
- 선택하지 않은 대안과 이유

과거 기록을 삭제하지 않고 현재 상태와 역사적 기록을 구분한다.

## 12. AI의 역할

AI는 다음 역할을 수행할 수 있다.

- 학습 보조
- 기술 조사
- 개발 계획
- 시스템 설계
- 코드 작성
- 코드 리뷰
- 오류 분석
- 테스트 계획
- 문서 작성
- 프로젝트 지식 검색
- 구현 대안 비교

AI가 제안한 내용을 사용자가 이해하고 검증한 후 프로젝트에 적용한다.
