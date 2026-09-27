# AI Development Workflow

## 1. 기본 Workflow

```text
목표 정의
  ↓
현재 상태 확인
  ↓
학습 / 조사
  ↓
구현 방법 결정
  ↓
설계
  ↓
최소 구현
  ↓
Build
  ↓
Run
  ↓
Test
  ↓
성공 → 기록
  ↓
실패 → 로그 수집 → 원인 분석 → 수정 → 재검증
```

## 2. 작업 시작 전

- 지금 무엇을 하려는가?
- 왜 필요한가?
- 현재 어디까지 되어 있는가?
- 어떤 파일/기능과 관련되는가?
- 어떤 자료를 기준으로 하는가?
- 내가 모르는 것은 무엇인가?
- AI가 판단해야 하는 부분은 무엇인가?

## 3. 모드 선택

- [LEARNING](modes/LEARNING.md)
- [RESEARCH](modes/RESEARCH.md)
- [DESIGN](modes/DESIGN.md)
- [IMPLEMENTATION](modes/IMPLEMENTATION.md)
- [DEBUG](modes/DEBUG.md)
- [REVIEW](modes/REVIEW.md)

복합 작업에서는 여러 모드를 순서대로 사용한다.

예:

```text
RESEARCH → DESIGN → IMPLEMENTATION → DEBUG → REVIEW
```

## 4. 모르는 상태에서 작업하기

```text
모름
→ AI에게 조사 요청
→ 가능한 방법 확인
→ 장단점 이해
→ 구현 방법 선택
→ 설계
→ 구현
```

## 5. AI-Hub와 Repository의 관계

프로젝트 관련 사실이 필요하면 실제 repository와 AI-Hub를 활용한다.

```text
AI 운영 지침
      ↓
프로젝트 지침
      ↓
Repository / 공식 자료
      ↓
AI-Hub를 통한 프로젝트 지식 검색
      ↓
현재 작업
```

AI-Hub는 원본 repository가 아니라 프로젝트 지식을 검색하고 연결하는 Knowledge Hub다. 실제 코드의 최신 상태는 repository를 기준으로 한다.

## 6. 구현 Loop

```text
구조 확인
→ 최소 코드 작성
→ Build
→ Run
→ Test
→ 결과 확인
→ 기능 추가
```

## 7. 오류 Loop

```text
오류 발생
→ 터미널 로그 확보
→ 실행 상태 확인
→ AI에 원본 로그 전달
→ 사실/추측 분리
→ 원인 후보 확인
→ 최소 수정
→ Build
→ Run
→ Test
```

## 8. 검증 Loop

다음은 서로 다른 상태다.

```text
코드 작성
≠ Build 성공
≠ Node 실행 성공
≠ Topic 정상 출력
≠ Robot 정상 동작
```

각 단계별로 확인한다.

## 9. 문서화

```text
목표
→ 변경 내용
→ 발생한 문제
→ 원인
→ 해결
→ 검증 결과
→ 남은 문제
→ 다음 작업
```

## 10. 완료 조건

- 필요한 코드가 작성되었다.
- Build가 성공했다.
- 실행이 성공했다.
- 기능이 실제로 검증되었다.
- 발생한 오류가 기록되었다.
- 현재 상태가 문서에 반영되었다.
- 다음 작업이 명확하다.
