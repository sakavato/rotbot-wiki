# 분야별 개념

[중심 설명으로](robotics_system_overview.md) · [처음으로](README.md)

중심 글에서 낯선 용어를 만났을 때 찾아보는 문서다. 물리적 직관과 전체 흐름은 [중심 본문](robotics_system_overview.md)에서 설명하며, 여기서는 필요한 분야만 읽으면 된다. 주요 용어는 한글(영문, 약어)로 병기하며, 번역이 다양한 용어는 영문을 기준으로 확인한다.

<a id="geometry"></a>
## 1. 몸의 구조와 움직임을 표현하는 언어

**링크**(link)는 몸체의 부분, **관절**(joint)은 그 사이의 연결이다. **자유도**(degrees of freedom, DOF)는 독립적으로 변할 수 있는 구성의 차원을 말한다. **말단 장치**(end effector)는 손·그리퍼 등 작업에 직접 사용하는 말단이다. 작업을 정의하는 변수와 로봇의 모든 관절 변수가 같지는 않다. 예를 들어 평면에서 두 회전 관절로 움직이는 팔은 두 관절각으로 구성을 표현한다. 손을 어디에 놓을지는 평면의 위치 두 값으로 표현할 수 있지만, 그 위치에서 손의 방향까지 임의로 정할 수 있는지는 별도 문제다.

| 키워드 | 의미 | 연결해서 읽을 것 |
|---|---|---|
| 구성(configuration) / 구성 공간(configuration space, C-space) | 로봇의 구성을 표현한 변수와 가능한 구성의 공간 | 관절 한계·충돌 제약, 휴머노이드의 떠 있는 몸체 |
| 관절 공간(joint space) | 관절 변수로 표현한 공간 | 관절 상태·명령과 기구학 |
| 작업 공간(task space) | 수행하려는 작업을 자연스럽게 표현하는 공간 | 손의 위치만 요구하는지, 방향까지 요구하는지 |
| 도달 가능 작업 영역(workspace) | 로봇 말단이 도달할 수 있는 위치·방향의 범위 | 링크 구조·관절 범위와 작업 요구 |
| 위치·방향을 합친 배치(pose) / 특수 유클리드 군(special Euclidean group, SE(3)) | 위치와 방향을 함께 표현한 강체의 배치 / 3차원 강체 변환의 공간 | 기준 좌표계와 시간 |

Task space와 workspace는 모두 ‘작업 공간’으로 번역되기도 한다. 둘을 같은 뜻으로 쓰면 “작업에서 원하는 것”과 “로봇이 도달할 수 있는 것”을 혼동한다. [Modern Robotics 2.5](https://modernrobotics.northwestern.edu/nu-gm-book-resource/2-5-task-space-and-workspace/)

**정기구학**(forward kinematics, FK)은 주어진 관절 구성에서 말단의 위치·방향을 구한다. **역기구학**(inverse kinematics, IK)은 목표 말단 배치를 만족하는 관절 구성을 찾는다. IK의 해는 없거나 여럿일 수 있다. 해를 하나 얻었다고 그 자세까지 충돌 없이 이동하는 경로까지 얻은 것은 아니다. [역기구학](https://modernrobotics.northwestern.edu/nu-gm-book-resource/inverse-kinematics-of-open-chains/)

**자코비안**(Jacobian)은 현재 구성에서 관절 속도와 말단의 선속도(linear velocity)·각속도(angular velocity)를 연결한다. 힘(force)과 관절 토크(joint torque)의 관계에도 등장한다. **특이점**(singularity)은 독립적으로 만들어낼 수 있는 말단의 순간 운동 방향이 줄어드는 구성이다. 행렬 표현에서는 이를 자코비안의 랭크 감소라고 한다. 따라서 목표 위치가 도달 범위에 있다는 사실만으로 원하는 방향의 순간 운동까지 자유롭게 만들 수 있다고 볼 수 없다. [Jacobian](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-1-1-space-jacobian/), [특이점](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-3-singularities/)

<a id="dynamics"></a>
### 기구학에서 동역학으로

힘·토크와 관성의 출발점은 [본문의 물체 들기 예](robotics_system_overview.md#vertical)다. 직선 운동에서는 물체에 작용하는 힘들을 합친 **알짜힘**(net force)과 가속도 사이에 `F = m × a`의 관계가 있다. `F`는 힘[N], `m`은 질량[kg], `a`는 가속도[m/s²]다. 같은 질량을 더 크게 가속하려면 더 큰 알짜힘이 필요하다. 물체를 정지 상태로 들고 있다면 가속도와 알짜힘은 0이지만, 손이 주는 위쪽 힘과 중력이 각각 0인 것은 아니다. 두 힘이 균형을 이루는 것이다. [뉴턴의 운동 법칙](https://openstax.org/books/university-physics-volume-1/pages/5-3-newtons-second-law)

**기구학**(kinematics)이 구성과 운동의 기하학적 관계를 다룬다면, **동역학**(dynamics)은 그 운동과 힘·토크의 관계를 다룬다. **역동역학**(inverse dynamics)은 주어진 운동과 외력 조건에서 필요한 관절 힘·토크를 구한다. **정동역학**(forward dynamics)은 주어진 힘·토크에서 운동의 변화를 구하며 시뮬레이션에 연결된다. [역동역학과 정동역학의 관계](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-3-newton-euler-inverse-dynamics/)

예를 들어 손이 같은 경로를 지나더라도 하중·자세·가속도가 달라지면 필요한 입력을 다시 검토해야 한다. 그래서 기구학적으로 가능한 자세, 동역학적으로 실행 가능한 움직임, 하드웨어가 실제로 만들 수 있는 출력은 구분해서 본다. 휴머노이드에서는 여기에 [몸체와 접촉 조건](#contact)이 더해진다.

<a id="estimation"></a>
## 2. 측정값에서 필요한 상태를 구한다

**측정값**(measurement)과 **상태 추정값**(state estimate)은 구분한다. 상태 추정값은 측정·모델·이전 정보로 구한 상태다. 로봇의 상태와 물체의 상태도 구분해야 한다. 예를 들어 카메라에 물체가 잠깐 가려져도 이전 위치·속도와 움직임 모델로 현재 위치를 예측할 수 있다. 하지만 그 예측을 새로 측정한 값처럼 취급해서는 안 되며, 다시 보였을 때 관측과 비교해 보정해야 한다. 칼만 필터(Kalman filter)는 예측과 측정의 불확실성을 고려해 상태를 갱신하는 대표적인 방법이다. 무엇을 상태 변수로 두는지에 따라 같은 도구로도 다른 문제를 푼다.

| 키워드 | 무엇을 알고 싶은가? | 연결 |
|---|---|---|
| 위치 추정(localization) | 기준 지도·좌표계에서 로봇이 어디에 있는가? | 계획의 시작 상태와 목표 좌표 |
| 주행 추정(odometry) / 시각 주행 추정(visual odometry, VO) / 시각·관성 주행 추정(visual-inertial odometry, VIO) | 시간에 따른 로봇·카메라의 움직임은 어떠한가? | 연속적인 이동 추정, 누적 오차, 시각·관성 정보 |
| 동시 위치 추정 및 지도 작성(simultaneous localization and mapping, SLAM) | 환경 지도와 그 안에서 움직이는 로봇의 상태는 무엇인가? | 관측 대응, 위치 추정, 지도 갱신 |
| 전단(front-end) | 관측에서 어떤 특징·대응·운동 정보를 만들 것인가? | 센서 전처리, 데이터 대응과 오측정 |
| 후단(back-end) | 관측 관계와 모델을 함께 만족하는 상태를 어떻게 추정할 것인가? | 필터링·최적화, 추정 불확실성 |
| 루프 폐합(loop closure) | 이전에 본 장소와 지금의 관측을 어떻게 연결하는가? | 누적 오차의 보정과 지도 일관성 |

필터링(filtering)·최적화(optimization)는 추정 방법의 구분이고, 위치 추정(localization)·지도 작성(mapping)은 풀려는 문제의 구분이다. [SLAM 개관 논문](https://arxiv.org/abs/1606.05830)

**물체 추적**(object tracking)은 관측 사이에서 물체의 상태와 동일성을 이어가는 문제다. SORT(Simple Online and Realtime Tracking)는 검출(detection) 결과, 칼만 필터, 데이터 연관(data association)을 결합한 사례다. 여기서 얻은 물체 추적 결과와 로봇 자신의 pose는 다른 정보이며, 제어에 쓰려면 출력의 좌표·차원·시각을 확인해야 한다. [SORT 원 논문](https://arxiv.org/abs/1602.00763)

휴머노이드의 몸체 상태는 시각 SLAM만으로 설명하지 않는다. 관성 측정 장치(inertial measurement unit, IMU), 관절 기구학, 접촉 정보를 융합해 pose·속도를 추정하는 접근도 있다. 발이 고정 접촉 중이라는 가정을 쓰는 추정기는 접촉의 변화와 그 유효성을 함께 다뤄야 한다. [Contact-aided InEKF](https://arxiv.org/abs/1904.09251)

<a id="planning"></a>
## 3. 목표에서 실행할 움직임으로

**작업 계획**(task planning)은 무엇을 어떤 순서로 할지, **동작 계획**(motion planning)은 몸을 어떻게 움직일지를 묻는다. 예를 들어 ‘손을 접근시킨다 → 잡는다 → 들어 올린다’는 작업의 순서이고, 접근하는 동안 팔과 손이 선반을 피하도록 구체적인 움직임을 정하는 것은 동작 계획이다.

**경로**(path)는 지나갈 구성의 연결, **궤적**(trajectory)은 시간에 따른 움직임이다. A*(A-star) 같은 그래프 탐색(graph search), RRT(rapidly-exploring random tree) 계열의 샘플링(sampling), 최적화는 경로·동작을 구하는 서로 다른 접근으로 읽는다. 탐색 결과를 반드시 특정 최적화기에 넣어야 한다는 고정 순서는 없다. [Motion planning 개요](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-1-overview-of-motion-planning/)

**시간 매개변수화**(time parameterization)는 경로를 어떤 시간 진행으로 실행할지 정하는 문제다. 시간 최적 경로 매개변수화(time-optimal path parameterization, TOPP)는 이 맥락에 놓인다. 단순히 곡선을 매끄럽게 만드는 보간(interpolation)과, 동역학·구동 제약 아래에서 실행 시간을 정하는 문제는 다르다. [Time-optimal time scaling](https://modernrobotics.northwestern.edu/nu-gm-book-resource/9-4-time-optimal-time-scaling-part-1-of-3/)

**최적 제어 문제**(optimal control problem, OCP)는 풀려는 문제, **비선형 계획법**(nonlinear programming, NLP)은 비선형 최적화 문제의 형식, **직접 콜로케이션**(direct collocation)은 연속시간 동역학을 이산 제약으로 다루는 수치적 접근이다. **모델 예측 제어**(model predictive control, MPC)는 현재 상태에서 유한 구간 문제를 반복해 풀고 일부 입력을 적용하는 제어 방식이다. 서로 같은 종류의 알고리즘 이름이 아니며, 계획과 제어의 경계에 함께 등장할 수 있다. [Trajectory optimization과 MPC](https://underactuated.mit.edu/trajopt.html)

<a id="control"></a>
## 4. 기준과 실제의 차이를 다룬다

**피드백**(feedback)은 관측된 결과를 다음 입력에 반영하고, **피드포워드**(feedforward)는 목표와 모델 등을 이용해 필요한 입력을 미리 구성한다. 두 방법을 함께 사용할 수 있다. 목표가 위치인지, 속도인지, 힘인지에 따라 비교하는 값과 하위 명령이 달라진다. 예를 들어 물체 무게의 모델로 중력을 버티는 입력을 미리 더하고, 실제 관절각과 목표각의 차이를 피드백으로 줄일 수 있다. 위치 제어(position control)·힘 제어(force control)·상호작용 제어(interaction control)는 연결된 기능이지만 동의어는 아니다. [제어 개요](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-1-control-system-overview/)

**비례·미분 제어**(proportional-derivative control, PD)는 위치 오차와 속도 오차를 함께 사용하는 방법이다. 정지한 목표로 이동할 때 위치 오차에 따른 항은 목표로 되돌리려는 역할을 하고, 속도에 따른 항은 움직임을 가라앉히는 **감쇠**(damping) 역할을 할 수 있다. 감쇠가 부족하면 목표를 지나쳤다가 되돌아오는 진동이 생길 수 있다. 반대로 오차를 줄이려는 반응을 무조건 크게 해도 구동 한계·지연·잡음 때문에 문제가 생길 수 있다. [PD 제어와 적용 조건](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-4-motion-control-with-torque-or-force-inputs-part-1-of-3/)

**힘 제어**(force control)는 환경에 가할 힘·토크를 다룬다. 말단의 힘과 관절 토크 사이에는 기구학적 관계가 있다. 힘 센서 피드백을 사용하는 구성과 사용하지 않는 구성을 구분한다. [힘 제어](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-5-force-control/)

**임피던스 제어**(impedance control)는 운동의 변화에 대한 힘의 응답을, **어드미턴스 제어**(admittance control)는 가해진 힘에 대한 운동의 응답을 구성하는 관점으로 읽으면 된다. 예를 들어 임피던스는 손이 목표에서 밀렸을 때 스프링처럼 되미는 힘을 만드는 직관으로, 어드미턴스는 측정한 외력에 따라 목표 위치·속도를 바꾸어 손이 물러나게 하는 직관으로 시작할 수 있다. 가상 질량(mass)·스프링(spring)·감쇠 같은 관계를 구현할 때 어떤 신호를 입력받고 무엇을 명령하는지가 중요하다. **혼합 운동·힘 제어**(hybrid motion–force control)는 서로 다른 방향의 운동·힘 목표를 구분해 다룬다. 모두 “힘을 일정하게 만든다”로 합치지 않는다. [Modern Robotics 교재 11.6–11.7](https://hades.mech.northwestern.edu/images/b/b2/MR-2up.pdf)

<a id="contact"></a>
## 5. 접촉·균형·전신 움직임

**접촉**(contact)은 장애물에 부딪히는 사건만을 뜻하지 않는다. 발의 지지와 손의 파지(grasping)는 작업을 가능하게 하는 접촉이다. **접촉 모드**(contact mode)는 어느 부위가 어떤 방식으로 접촉하는지의 구분이다. 접촉 전환은 연속적으로 움직이던 시스템의 제약을 바꿀 수 있다. [접촉과 hybrid dynamics](https://underactuated.mit.edu/contact.html)

**마찰 원뿔**(friction cone)은 단순한 Coulomb 모델에서 접촉이 미끄러지지 않도록 전달할 수 있는 힘의 범위를 표현한다. **렌치**(wrench)는 힘과 모멘트(moment)를 함께 나타낸다. **힘 폐쇄**(force closure)는 주어진 접촉·마찰 모델 아래에서 임의 방향의 외부 wrench에 저항할 수 있는 파지의 성질이다. 실제 구동 한계까지 자동으로 보장하지 않는다. [마찰과 접촉력](https://modernrobotics.northwestern.edu/nu-gm-book-resource/12-2-1-friction/), [Force closure](https://modernrobotics.northwestern.edu/nu-gm-book-resource/12-2-3-force-closure/)

| 키워드 | 첫 이해 | 함께 확인할 조건 |
|---|---|---|
| 질량중심(center of mass, CoM) | 몸 전체 질량 분포를 대표하는 질량중심 | 자세·하중 변화 |
| 압력중심(center of pressure, CoP) | 접촉면 압력의 작용 위치를 요약한 점 | 접촉면과 힘의 유효성 |
| 영 모멘트 점(zero moment point, ZMP) | 선택한 평면에서 접촉 wrench의 수평 모멘트가 0이 되는 점 | 평면·접촉·동역학 모델의 가정 |
| 질량중심 기반 동역학(centroidal dynamics) | 질량중심과 그 주위 운동량에 집중한 동역학 | 관절 수준 제약을 어떻게 추가할지 |
| 발디딤 계획(footstep planning) | 발을 언제 어디에 놓을지 정하는 문제 | 도달 범위·지형·몸의 움직임 |

CoM은 질량 분포의 중심이고, CoP는 접촉 압력이 어디에 실리는지를 나타내므로 서로 다른 점이다. 예를 들어 왼발보다 오른발에 더 큰 수직 힘이 실리면, 양발의 압력을 합쳐 본 CoP는 오른발 쪽으로 이동한다. 몸이 움직일 때 가속도와 회전의 영향으로 CoP가 CoM의 수직 아래에 놓이지 않을 수 있다. ZMP는 접촉력과 운동의 관계를 모멘트 조건으로 표현할 때 쓰며, 평평한 지면의 적절한 접촉 조건에서는 CoP와 일치한다. 특정 점 하나가 정해진 범위 안에 있다는 사실만으로 모든 동작의 실행 가능성을 보장하지 않는다.

**전신 제어**(whole-body control, WBC)는 손·몸통·발의 목표와 관절·접촉 제약을 함께 고려한다. 발이 미끄러지지 않으면서 손도 목표를 따라야 하는 이유는 [본문의 균형 설명](robotics_system_overview.md#humanoid)에서 이어 볼 수 있다.

용어들의 관계와 단순화의 한계는 [보행 로봇 강의노트](https://underactuated.mit.edu/humanoids.html)를 따른다. “CoM의 수직 투영이 지지 영역 안”이라는 정적 직관 하나로 동적 보행 전체를 판단하지 않는다.

<a id="hardware"></a>
## 6. 구동과 센싱은 알고리즘의 조건을 만든다

**구동기**(actuator)는 물리적 힘·움직임을 만들고, **전달계**(transmission)는 이를 관절에 전달한다. 감속기(speed reducer)는 속도와 토크를 바꾸며 마찰(friction)·관성(inertia) 등 동적 특성에도 영향을 준다. **역구동성**(backdrivability)은 외부 힘으로 구동계를 역으로 움직이기 쉬운 성질이다. 센서가 외력을 알아내는 것과 기계적으로 잘 역구동되는 것은 별개의 성질이다. [구동·감속·마찰](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-9-actuation-gearing-and-friction/)

모터 전류(motor current), 출력 관절 토크, 손끝의 힘은 서로 다른 물리량이다. 그 사이를 연결하려면 전달계·마찰·기구 모델이 필요하다.

센서 명세에서는 **측정 범위**(range), **분해능**(resolution), **불확실성**(uncertainty), **반복성**(repeatability)을 구별한다. 범위는 측정할 수 있는 값의 구간, 분해능은 구별할 수 있는 작은 변화, 불확실성은 측정 대상에 합리적으로 부여할 수 있는 값들의 퍼짐을 나타내는 수치, 반복성은 같은 조건에서 측정을 반복했을 때 결과가 얼마나 모이는지를 뜻한다. 예를 들어 0.1 N 간격으로 표시되는 센서라도 보정 오차가 있으면 같은 힘을 계속 잘못 표시할 수 있다. 작은 표시 간격과 실제 값에 가까운 측정은 별개다. 불확실성은 표준편차나 조건이 명시된 구간의 폭 등으로 표현할 수 있다. [측정 불확실성의 의미](https://physics.nist.gov/cuu/Uncertainty/glossary.html)

힘 센서의 측정 범위와 기계적 과부하 한계도 다르다. ATI의 문서는 보정 범위를 넘는 포화(saturation) 시 출력이 유효하지 않을 수 있음을 설명한다. 제품별 처리와 상태 신호는 별도 확인한다. [ATI F/T FAQ, §2](https://www.ati-ia.com/library/documents/FT_FAQ.pdf)

<a id="interfaces"></a>
## 7. SW 사이에서 정보의 의미를 보존한다

로봇의 수치에는 값 외에 기준이 필요하다. 카메라와 손의 위치를 같은 기준으로 바꾸는 이유는 [본문의 좌표계 예](robotics_system_overview.md#estimation)에서 설명한다. 위치에는 **좌표계**(coordinate frame), 관측에는 **타임스탬프**(timestamp), 추정에는 유효성과 불확실성의 의미가 붙는다. 예를 들어 ROS REP-105의 `odom`은 연속적이지만 누적 오차가 생길 수 있고, `map` 기준 pose는 위치 보정으로 불연속적으로 바뀔 수 있다. 이 규약을 쓰는 로봇이라면 플랫폼의 위치 점프를 실제 급격한 움직임으로 곧바로 해석하면 안 된다. [REP-105 원문](https://raw.githubusercontent.com/ros-infrastructure/rep/master/rep-0105.rst)

<a id="timing"></a>
관측 시각, 메시지를 받은 시각, 제어 입력을 실제로 적용한 시각은 다를 수 있다. **제어 주기**(control period)는 반복 계산 사이의 예정 간격이고, **마감시간**(deadline)은 해당 계산 결과가 쓰일 수 있도록 완료해야 하는 시점이다. 실행 시점의 흔들림인 **지터**(jitter)와 정보가 늦게 도착하는 **지연**(latency)도 구분한다. 계산값이 맞더라도 늦으면 과거 상태에 대한 보정이 될 수 있다. **실시간 운영체제**(real-time operating system, RTOS)는 실행 순서와 시간 요구를 관리하는 기반이며, 사용 사실만으로 전체 제어의 시간 조건이 보장되지는 않는다. [실시간 시스템의 요구](https://design.ros2.org/articles/realtime_background.html)

**서비스 품질**(quality of service, QoS)은 통신에서 신뢰성·이력·큐 등의 정책을 표현한다. 메시지가 전달됐다는 사실과 그 내용이 지금도 유효하다는 판단은 구분해야 한다. [ROS 2 QoS 설계](https://design.ros2.org/articles/qos.html)

**액션**(action)은 ROS 2에서 목표·피드백·결과와 취소를 다루는 장시간 작업 인터페이스다. [ROS 2 Actions 설계](https://design.ros2.org/articles/actions.html)
