# 분야별 개념 찾아보기

[오버뷰](robotics_system_overview.md) · [일자별 안내](learning_path.md) · [처음으로](README.md)

궁금한 질문에서 분야별 설명으로 들어간다. 각 글은 개념의 뜻과 예시, 다른 기능과의 연결을 다룬다. 전체 흐름은 [오버뷰](robotics_system_overview.md#system-map)에서 확인하고, 아래 글은 필요한 부분을 골라 읽으면 된다.

<a id="geometry"></a>
<a id="dynamics"></a>
## 기구학과 동역학

손의 위치와 관절각은 어떻게 연결되며, 움직이려면 어떤 힘이 필요한가?

[몸의 표현·정기구학·역기구학·자코비안](concepts/geometry.md#geometry) · [힘과 운동](concepts/geometry.md#dynamics)

<a id="hardware"></a>
<a id="motor-control"></a>
## 모터와 구동

명령이 실제 힘이 되기까지 무엇이 필요한가?

[모터 원리](concepts/actuation.md#motor-principle) · [모터 종류·서보](concepts/actuation.md#motor-types) · [감속비](concepts/actuation.md#gearing) · [전류·토크·역구동성](concepts/actuation.md#hardware) · [PWM](concepts/actuation.md#motor-control)

<a id="sensor-quality"></a>
## 촉각과 힘 센싱

무엇을 느끼고 측정하며, 그 숫자를 어느 조건에서 사용할 수 있는가?

[햅틱스](concepts/sensing.md#haptics) · [촉각·힘 센서의 차이](concepts/sensing.md#tactile-force) · [센싱 원리](concepts/sensing.md#transduction) · [정확도·정밀도·분해능](concepts/sensing.md#sensor-quality) · [응답 속도](concepts/sensing.md#sensor-response)

<a id="estimation"></a>
<a id="object-tracking"></a>
## 상태 추정과 물체 추적

측정값에서 로봇과 물체의 현재 상태를 어떻게 알아내는가?

[상태 추정·SLAM](concepts/estimation.md#estimation) · [시각·관성 추정](concepts/estimation.md#visual-inertial) · [검출·데이터 연관·추적](concepts/estimation.md#object-tracking) · [단일·다중 객체 추적](concepts/estimation.md#tracking-scope)

<a id="planning"></a>
## 작업 계획과 동작 계획

과업과 유지 목표를 어떤 움직임으로 바꾸는가?

[작업 단계와 움직임](concepts/planning.md#planning) · [경로·궤적](concepts/planning.md#path-trajectory) · [탐색·샘플링·최적화](concepts/planning.md#planning-methods) · [MPC](concepts/planning.md#optimization)

<a id="control"></a>
## 피드백과 상호작용 제어

목표를 따라 움직이거나 멈춰 있을 때 무엇을 보정하는가?

[피드백·피드포워드](concepts/control.md#control) · [목표 유지·PD](concepts/control.md#setpoint) · [경로·궤적 추종](concepts/control.md#tracking) · [힘·임피던스·어드미턴스 제어](concepts/control.md#interaction)

<a id="contact"></a>
<a id="floating-base"></a>
<a id="whole-body-control"></a>
## 접촉과 전신 균형

물체를 잡으면서 서 있으려면 무엇을 함께 고려해야 하는가?

[부유 기저](concepts/contact.md#floating-base) · [마찰·파지](concepts/contact.md#friction) · [CoM·CoP·ZMP](concepts/contact.md#balance) · [전신 제어·목표 우선순위](concepts/contact.md#whole-body-control)

<a id="interfaces"></a>
<a id="timing"></a>
## 좌표계·시각·SW 인터페이스

같은 숫자와 완료 신호를 기능마다 다르게 해석하지 않으려면?

[좌표계·단위](concepts/interfaces.md#interfaces) · [주기·지연·지터](concepts/interfaces.md#timing) · [QoS·Action·작업 결과](concepts/interfaces.md#communication)

<a id="further-study"></a>
## 필요할 때 더 깊게 읽고 확장할 주제

현재 설명에서 더 나아갈지는 **풀고 싶은 질문이 생겼는가**를 기준으로 정하면 된다. 아래는 관련 개념을 묶은 선택 안내이며, 정해진 이수 순서나 작성 예정 목록은 아니다. 문서를 확장할 때도 필요한 질문을 골라 해당 개념에 설명·예시·근거를 덧붙일 수 있다.

| 주제·현재 설명 범위 | 더 깊게 알아볼 질문 | 이어 읽을 자료 |
|---|---|---|
| **기구학·자코비안**: [관절 구성, 손의 움직임, 특이점](concepts/geometry.md#geometry) | 자세에 따라 손을 움직이기 쉬운 방향이 어떻게 달라지는가? 이를 나타내는 조작성(manipulability)은 어떻게 계산하는가? | Modern Robotics [5.4 조작성](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-4-manipulability/) |
| **구동·전달계**: [모터 전류, 관절 토크, 손끝 힘의 관계](concepts/actuation.md#hardware) | 감속비·마찰·모터 관성이 관절의 응답에 얼마나 영향을 주는가? | Modern Robotics [8.9 구동·감속·마찰](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-9-actuation-gearing-and-friction/) |
| **센서 측정 품질**: [정확도·정밀도·분해능·불확실성](concepts/sensing.md#sensor-quality) | 여러 측정값으로 힘이나 위치를 계산할 때 각 값의 불확실성을 어떻게 합치는가? | NIST [불확실성 성분의 결합](https://physics.nist.gov/cuu/Uncertainty/combination.html) |
| **동작·궤적 계획**: [경로, 시간 계획, 구동 제약](concepts/planning.md#planning) | 주어진 경로를 토크 한계 안에서 얼마나 빠르게 실행할 수 있는가? | Modern Robotics [9.4 시간 최적화](https://modernrobotics.northwestern.edu/nu-gm-book-resource/9-4-time-optimal-time-scaling-part-1-of-3/) |
| **접촉 제어**: [힘·임피던스·어드미턴스 제어의 목표](concepts/control.md#control) | 물체를 얼마나 강하게 누르거나 외력에 얼마나 부드럽게 반응하게 할 것인가? 그 관계를 어떻게 식으로 표현하는가? | MIT [Manipulator Control](https://manipulation.mit.edu/force.html) |
| **SLAM·상태 추정**: [전단·후단, 관측 대응, 위치·지도 추정](concepts/estimation.md#estimation) | 잘못된 관측 대응과 누적 오차를 어떻게 다루며, 장소를 다시 보았을 때 지도와 위치를 어떻게 보정하는가? | [SLAM 개관 논문](https://arxiv.org/abs/1606.05830)의 데이터 연관·추정·루프 폐합 설명 |
| **물체 추적**: [검출·데이터 연관·상태 갱신](concepts/estimation.md#object-tracking) | 물체를 놓치는 오류와 다른 물체로 바꿔 따라가는 오류를 어떻게 구분하고 평가하는가? | [SORT](https://arxiv.org/html/1602.00763) §4의 평가 지표와 비교 |
| **균형·전신 제어**: [접촉 조건, 유지 목표, 우선순위·제약](concepts/contact.md#contact) | 손·몸통·발의 목표가 충돌할 때 우선순위와 가중치를 쓰는 방법은 어떤 결과 차이를 만드는가? | [TALOS 전신 제어 비교 연구](https://www.frontiersin.org/journals/robotics-and-ai/articles/10.3389/frobt.2022.826491/full) |

각 자료는 표의 질문에 해당하는 부분부터 읽으면 된다. 수식 유도·알고리즘 구현·제품별 설정은 실제 학습이나 적용에 필요한 깊이까지 선택한다.
