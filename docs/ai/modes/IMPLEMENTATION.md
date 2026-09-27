# AI Mode — Implementation

## 목적

검토된 설계를 실제 repository 코드로 구현한다.

## 원칙

- 현재 프로젝트를 확인하지 않고 코드를 임의로 작성하지 않는다.
- 불필요한 구조 변경을 하지 않는다.
- 가능한 최소 변경으로 구현한다.

## 권장 프롬프트

```text
[구현]

현재 프로젝트의 관련 파일과 구조를 먼저 확인해줘.

이번 목표:
<목표>

설계:
<선택한 설계>

기존 구조를 최대한 유지하면서 필요한 부분만 수정해줘.

구현 전에:
1. 수정할 파일
2. 수정 이유
3. 기존 코드와의 연결
4. 예상되는 동작

을 설명해줘.

그 다음 코드를 작성하고 다음을 알려줘.

- Build 명령
- Source 명령
- Run 명령
- Test 방법
- 정상 결과
```

## 기본 검증

```bash
colcon build --packages-select <package>
source install/setup.bash
```

필요하면:

```bash
ros2 node list
ros2 topic list
ros2 topic info <topic>
ros2 topic echo <topic>
```

## 완료 조건

코드 작성뿐 아니라 Build, Run, Test까지 수행하여 실제 동작을 확인한다.
