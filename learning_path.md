# 교육 구성과 일자별 읽기 안내

[처음으로](README.md) · [휴머노이드 전체 설명](robotics_system_overview.md) · [분야별 개념](concepts.md)

이 자료는 작성자가 **5일 사내 로봇 교육을 청강하며 남긴 키워드 메모**를 정리하는 과정에서 시작됐다. 이후 같은 교육을 듣는 수강생이 어떤 분야를 접하는지 미리 살펴보고, 수강 후에는 배운 용어를 로봇 전체 동작과 연결해 복습할 수 있도록 구성했다.

아래의 **교육 주제·키워드**는 당시 메모에서 교육의 큰 구성을 보여주는 내용을 선별해 요약한 것이다. **전체 동작과의 연결·읽을 곳**은 이해를 돕기 위해 이 자료에서 덧붙인 해설이다. 공식 강의계획서 전체를 재현한 것은 아니며, 확인할 수 있는 교육 기록을 바탕으로 안내한다. 세부 키워드는 [내용 선정 기준](README.md#scope)에 따라 필요한 만큼 다룬다.

## 교육 구성 한눈에 보기

교육에는 로봇의 몸과 운동을 표현하는 기초, 실제 힘을 만드는 구동, 접촉을 알아내는 센싱, 동작 계획과 제어, 영상으로 물체와 로봇의 상태를 알아내는 내용이 함께 기록되어 있다. 이 위키의 오버뷰에서는 이 여러 분야를 **서서 물체를 집는 휴머노이드**의 예로 연결했다.

| 일자 | 메모에 기록된 교육 주제 | 전체 동작과 연결해서 볼 부분 |
|---|---|---|
| [1일차](#day-1) | 로봇 공학 기초, 기구학, 햅틱스, SLAM 개론 | 몸의 구조·움직임을 표현하고, 접촉 감각과 위치·지도 추정의 문제를 접한다. |
| [2일차](#day-2) | 전기 모터의 원리·종류, 감속기, 구동 회로와 제어 | 명령을 실제 힘·움직임으로 바꾸는 과정과 구동 능력을 살펴본다. |
| [3일차](#day-3) | 센서 특성, 촉각 센싱, 힘·토크 측정과 활용 | 물리적 접촉이 측정값이 되는 과정과, 그 값을 해석할 조건을 살펴본다. |
| [4일차](#day-4) | 로봇 모델, 경로·궤적 계획, 추종 제어, 상호작용 제어. 첨부: 로봇 제어 파이프라인 도식 | 목표에서 실행할 움직임을 정하고, 실제 상태·접촉에 맞게 제어하는 관계를 살펴본다. |
| [5일차](#day-5) | 시각 객체 추적과 시각 SLAM | 물체의 상태를 이어서 추적하는 문제와, 로봇 자신의 위치·지도를 구하는 문제를 구분한다. |

일자는 **교육을 따라 찾아가는 기준**이고, 오버뷰의 1~7절은 **전체 동작을 이해하기 위한 설명 순서**다. 두 번호는 서로 대응하지 않는다. 1일차와 4일차에 기구학이, 1일차와 5일차에 SLAM이 다시 등장하듯 같은 주제가 여러 날에 걸쳐 연결된다.

<a id="day-1"></a>
## 1일차 — 로봇 공학 기초·햅틱스·SLAM

메모의 앞부분은 링크(link)·관절(joint)·말단 장치(end effector) 등 로봇의 구성 요소와 **기구학**(kinematics)을 다룬다. 자유도(degrees of freedom, DOF), 관절 공간(joint space)과 작업 공간(task space), 정기구학(forward kinematics, FK)·역기구학(inverse kinematics, IK), 자코비안(Jacobian)이 기록되어 있다.

이어 **햅틱스**(haptics)에서 사람의 촉각·운동 감각, 감각 피드백과 그리퍼 등의 연구 사례를 다룬다. **동시 위치 추정 및 지도 작성**(simultaneous localization and mapping, SLAM)도 별도 주제로 기록되어 있으며, 센서별 구분, 전단(front-end)·후단(back-end), 필터링(filtering)·최적화(optimization) 접근을 포함한다.

**전체 동작과의 연결:** 손을 어디로 보낼지 정하려면 몸의 구조와 현재 상태를 표현할 수 있어야 한다. 접촉을 통해 얻는 정보와 위치·지도 추정은 각각 무엇을 알아내는지 구분해서 본다. 1일차 전체를 기구학만 배운 날로 축약하면 햅틱스와 SLAM의 맥락이 빠진다.

**읽을 곳:** 오버뷰 [1절: 위치·상태](robotics_system_overview.md#estimation), [2절: 자세·움직임](robotics_system_overview.md#geometry), [5절: 접촉](robotics_system_overview.md#contact). 용어는 [몸의 표현](concepts.md#geometry)과 [상태 추정](concepts.md#estimation)에서 보충한다.

<a id="day-2"></a>
## 2일차 — 전기 모터와 구동

전기 모터(electric motor)가 힘을 만드는 원리에서 시작해 전류(current)·토크(torque)·속도(speed)·전력(power), 감속기(speed reducer)와 기어비(gear ratio)를 다룬다. 모터 종류별 특성과 선택 기준도 함께 기록되어 있다.

구동·제어 항목에는 피드백(feedback)·피드포워드(feedforward), 구동 회로와 서보 모터(servo motor)가 있다. 로봇의 적용 예를 통해 관절에 맞는 모터를 선택하는 문제로 이어진다.

**전체 동작과의 연결:** 계획이나 제어가 명령한 값은 구동계를 거쳐 실제 힘과 움직임이 된다. 이 날의 내용을 통해 모터·전달계의 특성이 어떤 동작을 수행할 수 있는지와 연결된다는 점을 살펴볼 수 있다. 회로를 구동하는 방식과 제어하려는 물리량의 구분은 [교육 메모의 정정과 보충](curriculum_coverage_and_gaps.md#corrections)에서 보충한다.

**읽을 곳:** 오버뷰 [3절: 힘·구동 능력](robotics_system_overview.md#vertical), [4절: 피드백](robotics_system_overview.md#closed-loop), 개념 문서의 [구동과 센싱](concepts.md#hardware).

<a id="day-3"></a>
## 3일차 — 힘·촉각 센싱

센서(sensor)가 물리량을 신호와 숫자로 바꾸는 과정, 센서의 종류와 특성을 다룬다. 측정 범위(range)·분해능(resolution)·정밀도(precision)·정확도(accuracy) 같은 정적 특성과 응답 시간(response time) 같은 동적 특성이 기록되어 있다.

촉각 센싱(tactile sensing)에서는 저항식(resistive)·압전식(piezoelectric)·정전용량식(capacitive)·광학식(optical) 원리와 배열·소재·구현 사례를 다룬다. 힘·토크 센서(force/torque sensor, F/T sensor)의 구조와 센서 사양 해석도 포함된다. 활용 예에는 물체 인식, 힘 제어, 파지 안정성, 미끄럼 검출 등이 있다.

**전체 동작과의 연결:** 1일차의 햅틱스가 감각과 피드백의 관점을 포함했다면, 이 날의 메모는 접촉을 측정하는 장치와 측정 품질을 더 구체적으로 다룬다. 물체를 잡았는지, 미끄러지는지 판단하려면 어떤 값이 측정되며 그 값이 언제 유효한지 알아야 한다.

**읽을 곳:** 오버뷰 [5절: 접촉과 파지](robotics_system_overview.md#contact), 개념 문서의 [센서 범위·분해능·불확실성](concepts.md#hardware), [적용 사례: 손은 닫혔는데 왜 물체를 놓쳤을까?](platform_robot_walkthrough.md).

<a id="day-4"></a>
## 4일차 — 동작 계획과 제어

**로봇 제어 파이프라인**(robot control pipeline)은 **4일차 동작계획의 첨부 자료**다. 이 날의 계획·제어 관계를 살펴보는 도식이며, 5일 교육 전체의 구성도를 뜻하지 않는다. 위키의 오버뷰는 여러 일자의 주제와 별도 기술 자료를 종합해 전체 동작을 설명한다.

4일차 메모의 기초 부분은 구성 공간(configuration space, C-space)·작업 공간, 기구학·동역학(dynamics), 구동·센싱 구성에 따른 제어의 조건을 다룬다.

계획 부분에는 **경로 계획**(path planning)을 위한 그래프 탐색(graph search)·샘플링(sampling)·최적화, **궤적 계획**(trajectory planning)을 위한 보간(interpolation)·시간 매개변수화(time parameterization)가 기록되어 있다. 제어 부분에는 **추종 제어**(tracking control), 위치·속도·토크를 다루는 제어 방식, **상호작용 제어**(interaction control)의 임피던스(impedance)·어드미턴스(admittance)·혼합 위치·힘 제어(hybrid position–force control)가 있다. 팔·다리형 로봇 등의 적용 사례도 포함된다.

**전체 동작과의 연결:** 1일차의 몸 표현, 2일차의 구동, 3일차의 측정이 목표 움직임의 계획과 실행에서 만난다. 여러 알고리즘 이름을 모두 차례로 실행하는 단계로 읽기보다, 경로를 구하는 방법인지, 시간을 정하는 방법인지, 실행 중 입력을 조절하는 방법인지 구분하면 연결을 이해하기 쉽다.

**읽을 곳:** 오버뷰 [2절: 자세·움직임 계획](robotics_system_overview.md#geometry)부터 [7절: 작업 통합](robotics_system_overview.md#integration)까지의 관련 설명, 개념 문서의 [계획](concepts.md#planning)·[제어](concepts.md#control)·[접촉과 균형](concepts.md#contact).

<a id="day-5"></a>
## 5일차 — 시각 객체 추적과 시각 SLAM

5일차는 **시각 객체 추적**(visual object tracking)과 **시각 SLAM**(visual SLAM)의 두 메모로 남아 있어 나누어 안내한다.

### 물체의 상태를 이어가는 시각 객체 추적

필터링을 이용한 상태 추정, 추적 성능 평가, 딥러닝 기반 추적을 다룬다. 단일 객체 추적(single-object tracking)에서 다중 객체 추적(multiple-object tracking, MOT)과 3차원 추적까지 범위를 넓히며, 검출(detection)·데이터 연관(data association)·분할(segmentation)이 추적에 사용되는 사례를 포함한다.

**전체 동작과의 연결:** 로봇이 다룰 물체가 움직이거나 잠시 가려질 때도 그 물체의 상태와 동일성을 이어서 파악하는 문제와 연결된다. 4일차의 ‘추종 제어’는 몸이 목표 움직임을 따르게 하는 문제이므로, 같은 tracking이라는 단어가 등장해도 구분해야 한다.

### 로봇의 위치와 지도를 구하는 시각 SLAM

카메라·라이다(LiDAR)·관성 측정 장치(inertial measurement unit, IMU) 등 센서, 전단의 특징 추출(feature extraction)·대응·운동 추정, 후단의 필터링·최적화·지도 관리가 기록되어 있다. 루프 폐합(loop closure), 시각·관성 주행 추정(visual-inertial odometry, VIO), 지도 표현, 운동으로부터의 구조 복원(structure from motion, SfM)과 관련 도구·연구 사례도 포함한다.

**전체 동작과의 연결:** 1일차에도 등장한 위치·지도 추정 문제를 센서·기하·확률·최적화와 개별 방법까지 연결해 살펴볼 수 있다. 물체 추적이 대상 물체의 상태를 다룬다면, 여기서는 로봇·카메라 자신의 움직임과 환경 지도의 관계에 주목한다.

**읽을 곳:** 오버뷰 [1절: 위치·상태 파악](robotics_system_overview.md#estimation), 개념 문서의 [추정](concepts.md#estimation)·[좌표계와 시각](concepts.md#interfaces), [적용 사례: 손은 닫혔는데 왜 물체를 놓쳤을까?](platform_robot_walkthrough.md).

## 이 안내와 나머지 문서를 함께 읽는 방법

수강 전에는 위 표와 각 일자의 **교육 내용·전체 동작과의 연결**을 읽어 다룰 분야를 먼저 확인한다. 수강 중이나 수강 후에는 해당 일자의 링크로 오버뷰와 개념 설명을 찾아가면 된다. 로봇 전체 원리를 먼저 이해하고 싶다면 [오버뷰](robotics_system_overview.md)부터 읽고, 여기서 교육 키워드의 위치를 확인해도 된다.

용어의 뜻을 찾아볼 때는 [분야별 개념](concepts.md)을 사용한다. 이 문서는 전체 동작을 이해하는 데 필요한 개념을 선택해 보충한다. 감각 생리의 세부, 모터·촉각 센서의 구현, 개별 추적·SLAM 알고리즘의 계보 등은 구체적인 학습 필요가 생길 때 선택해서 살펴본다. 여러 날에 등장한 용어는 공통 설명으로 연결하고, 각 일자에서는 그 용어가 어떤 주제와 함께 다뤄졌는지 설명한다.

현재 오버뷰의 휴머노이드 집기 예, 유지 목표, 균형·전신 제어를 잇는 설명은 교육 메모를 이해하기 위해 재구성·보충한 내용이다. 원본 표현의 교정과 보완 범위는 [교육 메모의 정정과 보충](curriculum_coverage_and_gaps.md)에, 기술적 근거는 [출처 목록](references.md)에 정리했다.
