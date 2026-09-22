# 좌표계·시각·SW 인터페이스

[개념 찾아보기](../concepts.md) · [오버뷰](../robotics_system_overview.md) · [일자별 안내](../learning_path.md)

기능 사이에 전달하는 숫자의 기준과 시각, 작업 완료의 의미를 맞춘다.

[정보의 기준](#interfaces) · [시간 조건](#timing) · [통신·작업 결과](#communication)

<a id="interfaces"></a>
## SW 사이에서 정보의 의미를 보존한다

로봇의 수치에는 값 외에 기준이 필요하다. 카메라와 손의 위치를 같은 기준으로 바꾸는 이유는 [본문의 좌표계 예](../robotics_system_overview.md#estimation)에서 설명한다. 위치에는 **좌표계**(coordinate frame), 관측에는 **타임스탬프**(timestamp), 추정에는 유효성과 불확실성의 의미가 붙는다. 예를 들어 ROS REP-105의 `odom`은 연속적이지만 누적 오차가 생길 수 있고, `map` 기준 pose는 위치 보정으로 불연속적으로 바뀔 수 있다. 이 규약을 쓰는 로봇이라면 플랫폼의 위치 점프를 실제 급격한 움직임으로 곧바로 해석하면 안 된다. [REP-105 원문](https://raw.githubusercontent.com/ros-infrastructure/rep/master/rep-0105.rst)

<a id="timing"></a>
## 측정과 실행의 시각을 구분한다

관측 시각, 메시지를 받은 시각, 제어 입력을 실제로 적용한 시각은 다를 수 있다. **제어 주기**(control period)는 반복 계산 사이의 예정 간격이고, **마감시간**(deadline)은 해당 계산 결과가 쓰일 수 있도록 완료해야 하는 시점이다. 실행 시점의 흔들림인 **지터**(jitter)와 정보가 늦게 도착하는 **지연**(latency)도 구분한다. 계산값이 맞더라도 늦으면 과거 상태에 대한 보정이 될 수 있다. **실시간 운영체제**(real-time operating system, RTOS)는 실행 순서와 시간 요구를 관리하는 기반이며, 사용 사실만으로 전체 제어의 시간 조건이 보장되지는 않는다. [실시간 시스템의 요구](https://design.ros2.org/articles/realtime_background.html)

예를 들어 물체 위치 정보가 `x = 0.30`만 전달되면, 단위가 m인지 cm인지, 어느 좌표계인지, 언제 관측한 값인지 알 수 없다. 계획에 필요한 정보는 값과 함께 단위·좌표계·관측 시각·유효성을 해석할 수 있는 형태여야 한다. 그 정보가 메시지 안에 들어가는지, 인터페이스 규약으로 정해져 있는지는 구현에 따라 다르다.

<a id="communication"></a>
## 통신과 작업 결과의 의미

**서비스 품질**(quality of service, QoS)은 통신에서 신뢰성·이력·큐 등의 정책을 표현한다. 메시지가 전달됐다는 사실과 그 내용이 지금도 유효하다는 판단은 구분해야 한다. [ROS 2 QoS 설계](https://design.ros2.org/articles/qos.html)

**액션**(action)은 ROS 2에서 목표·피드백·결과와 취소를 다루는 장시간 작업 인터페이스다. [ROS 2 Actions 설계](https://design.ros2.org/articles/actions.html)

다만 ‘완료’의 뜻은 정의한 작업에 따라 달라진다. 손가락을 목표 각도로 닫는 작업의 완료와 물체를 성공적으로 집는 작업의 완료는 서로 다른 조건이다. 명령이 처리되었다는 알림, 측정한 손의 상태, 물체를 보유했다는 판단을 구분하는 과정을 [적용 사례](../platform_robot_walkthrough.md#grasp-case)에서 이어 읽을 수 있다.
