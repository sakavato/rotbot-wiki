# 보충: 관측된 현상에서 로봇 내부를 이해하기

[중심 설명으로](robotics_system_overview.md) · [분야별 개념](concepts.md)

다음은 측정(measurement)·추정(estimation)·제어(control)의 차이를 이해하기 위한 가상 예다. 관측된 현상은 원인을 생각하는 출발점이며, 그 자체로 진단 결과는 아니다.

| 현상 | 구분해서 생각할 것 | 관련 개념 |
|---|---|---|
| 로봇 위치가 갑자기 바뀐다 | 몸이 실제로 움직였는지, 위치 추정(localization)이 보정됐는지 | [좌표계(coordinate frame)와 위치 추정](concepts.md#interfaces) |
| 손의 위치 오차(position error)가 줄지 않는다 | 목표가 도달 가능한지, 기준 좌표가 맞는지, 필요한 출력을 만들 수 있는지 | [기구학(kinematics)](concepts.md#geometry), [구동(actuation)](concepts.md#hardware) |
| 손은 닫혔지만 물체를 놓친다 | 손의 구성, 접촉 여부, 물체 보유는 각각 다른 상태라는 점 | [접촉(contact)과 파지(grasping)](concepts.md#contact) |
| 힘 센서값이 계속 같다 | 힘이 일정한지, 센서가 포화(saturation)됐는지, 오래된 값인지 | [센서의 범위·유효성](concepts.md#hardware) |

예를 들어 ROS REP-105를 사용하는 구성에서 지도 기준 위치는 추정 보정으로 불연속적으로 바뀔 수 있다. 위치 점프만 보고 로봇의 급격한 움직임이라고 결론 내리기 전에 값의 기준을 확인해야 한다. [REP-105](https://raw.githubusercontent.com/ros-infrastructure/rep/master/rep-0105.rst)

또한 힘 센서가 측정 범위(range)를 넘으면 출력이 유효하지 않을 수 있다. “값이 안정적이다”와 “힘을 잘 제어하고 있다”를 구분하려면 측정의 유효성을 함께 알아야 한다. [ATI F/T FAQ](https://www.ati-ia.com/library/documents/FT_FAQ.pdf)

어떤 현상을 설명할 때 **측정값 → 추정한 상태 → 동작에 대한 판단** 중 어디에서 의미가 달라지는지 짚어 보면, 해당 분야 전문가에게 필요한 질문을 구체화할 수 있다.
