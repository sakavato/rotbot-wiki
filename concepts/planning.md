# 작업 계획과 동작 계획

[개념 찾아보기](../concepts.md) · [오버뷰](../robotics_system_overview.md) · [일자별 안내](../learning_path.md)

과업과 유지 목표를 실행할 움직임으로 구체화한다.

[작업과 동작](#planning) · [경로·궤적](#path-trajectory) · [계획 방법](#planning-methods) · [최적화·MPC](#optimization)

<a id="planning"></a>
## 목표에서 실행할 움직임으로

### 과업과 계속 유지할 조건을 함께 전달한다

**작업 계획**(task planning)은 무엇을 어떤 순서로 할지 정한다. **작업 실행 관리**(task execution)는 관측한 결과로 다음 단계에 갈지, 현재 목표를 유지할지, 다시 시도할지 판단한다. **동작 계획**(motion planning)은 그 목표를 달성할 자세·경로·시간·접촉 조건을 구체화한다.

예를 들어 ‘접근 → 잡기 → 들기’는 작업 단계의 순서다. 손이 선반을 피하면서 물체에 접근하도록 움직임을 정하는 일은 동작 계획이다. 여기에 **서기·균형 유지라는 목표도 함께 유효하다.** 집기 요청이 없을 때는 서기 목표가 남고, 집기 요청이 들어오면 손의 목표가 더해진다. 이러한 목표를 몸 전체에서 조정하는 방법은 [전신 제어](contact.md#whole-body-control)로 이어진다. [오버뷰의 작업 진행 예](../robotics_system_overview.md#integration)

<a id="path-trajectory"></a>

### 같은 길을 지나도 실행할 움직임은 달라진다

**경로**(path)는 지나갈 구성의 연결, **궤적**(trajectory)은 시간에 따른 움직임이다. 선반 옆을 돌아가는 같은 경로라도 1초에 이동할지 3초에 이동할지에 따라 필요한 속도·가속도가 달라진다. 그러므로 충돌 없는 경로를 얻은 뒤에도 시간과 구동 한계를 함께 검토해야 한다. 경로를 찾는 방법과 시간을 정하는 방법의 역할을 구분해서 읽는다. [Motion planning 개요](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-1-overview-of-motion-planning/)

**보간**(interpolation)은 주어진 지점 사이의 값을 정해 연결하는 방법이다. 예를 들어 관절이 거칠 각도와 통과 시각을 정하고, 그 사이의 목표 각도를 함수로 구해 궤적을 만들 수 있다. 어느 차수까지 매끄럽게 연결되는지는 보간 방법과 경계 조건에 따라 달라진다. [경유점을 잇는 보간 궤적](https://modernrobotics.northwestern.edu/nu-gm-book-resource/9-3-polynomial-via-point-trajectories/)

**시간 매개변수화**(time parameterization)는 주어진 경로 위에서 시간에 따라 얼마나 진행할지 정하는 문제다. **시간 최적 경로 매개변수화**(time-optimal path parameterization, TOPP)는 구동·동역학 제약 아래에서 실행 시간을 줄이는 경우다. 보간으로 궤적을 연결하는 것만으로 이런 제약이 만족되지는 않는다. [Time-optimal time scaling](https://modernrobotics.northwestern.edu/nu-gm-book-resource/9-4-time-optimal-time-scaling-part-1-of-3/)

<a id="planning-methods"></a>
### 그래프 탐색·샘플링·최적화는 어떻게 다른가?

손을 선반 옆으로 돌아가게 하는 문제를 생각하자. 아래 방법은 같은 문제에 서로 다른 방식으로 접근하며, 결합해서 쓰기도 한다.

| 접근 | 하는 일 | 로봇에서의 의미 |
|---|---|---|
| **그래프 탐색**(graph search) | 후보 상태와 이동 가능 연결을 노드·간선으로 표현하고, 시작에서 목표까지의 연결을 찾는다. | A*(A-star) 등으로 어떤 후보 자세들을 거칠지 찾는다. |
| **샘플링 기반 계획**(sampling-based planning) | 구성 공간에서 후보 자세를 뽑고 충돌 없는 연결을 확인해 그래프·트리를 만든다. | 모든 자세를 격자로 나누기 어려울 때 필요한 연결을 탐색한다. PRM·RRT가 대표적인 예다. |
| **최적화 기반 계획**(optimization-based planning) | 경로·궤적 등의 변수를 조정해 비용을 줄이면서 충돌·구동 제약을 만족시키려 한다. | 이동 거리나 시간을 줄일 수 있지만, 문제와 초기 후보에 따라 해를 찾지 못하거나 국소 해에 머물 수 있다. |

**PRM**(probabilistic roadmap)은 샘플링으로 연결망을 만든 뒤 그 위에서 A* 같은 탐색을 사용할 수 있다. **RRT**(rapidly-exploring random tree)는 뽑은 후보 방향으로 탐색 트리를 확장한다. 따라서 이 방법들은 서로 완전히 분리된 분류도, 반드시 순서대로 실행하는 단계도 아니다. [PRM과 그래프 탐색](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-5-sampling-methods-for-motion-planning-part-1-of-2/) · [RRT](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-5-sampling-methods-for-motion-planning-part-2-of-2/) · [최적화와 제약](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-7-nonlinear-optimization/)

<a id="optimization"></a>

### 선택 보충: 최적화와 MPC는 어떤 관계인가?

최적화(optimization)는 후보 중 무엇을 더 좋게 볼지 정한 기준과, 반드시 만족해야 할 조건을 함께 사용해 답을 구하는 방법이다. 예를 들어 ‘이동 시간을 줄이되 충돌과 토크 한계를 피하는 움직임’을 찾는 문제를 생각할 수 있다.

**모델 예측 제어**(model predictive control, MPC)는 현재 상태에서 앞으로 일정 구간의 움직임과 입력을 계산하고, 그중 처음 일부만 적용한 뒤 새 상태로 다시 계산한다. 관측이 다음 계산의 출발점이 되므로 계획 계산을 피드백에 사용하는 방식이다. ‘미리 한 번 계산한 궤적을 끝까지 실행한다’는 구성과 구분하면 된다.

논문에서 아래 용어를 만나면 역할을 구분하는 정도로 읽고, 수치적 유도는 필요할 때 살펴본다.

| 용어 | 구분할 역할 |
|---|---|
| 최적 제어 문제(optimal control problem, OCP) | 운동의 변화와 제약을 지키면서 어떤 상태·입력을 선택할지 정하는 문제 |
| 비선형 계획법(nonlinear programming, NLP) | 목적이나 제약에 비선형 관계를 포함하는 최적화 문제를 다루는 분야. OCP를 계산 가능한 형태로 바꿀 때 등장할 수 있다. |
| 직접 콜로케이션(direct collocation) | 연속적인 운동을 구간별로 표현하고 정해진 지점에서 동역학 관계를 만족시키도록 최적화 문제를 구성하는 수치적 방법 |

이 이름들은 로봇에서 반드시 순서대로 실행하는 단계가 아니다. [궤적 최적화·직접 콜로케이션·MPC](https://underactuated.mit.edu/trajopt.html)
