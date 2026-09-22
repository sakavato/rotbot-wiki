# 피드백과 상호작용 제어

[개념 찾아보기](../concepts.md) · [오버뷰](../robotics_system_overview.md) · [일자별 안내](../learning_path.md)

목표와 실제 상태를 비교해 입력을 조절하고, 접촉할 때 힘과 움직임을 함께 다룬다.

[피드백·피드포워드](#control) · [목표 유지·PD](#setpoint) · [힘·상호작용 제어](#interaction)

<a id="control"></a>
## 기준과 실제의 차이를 다룬다

**피드백**(feedback)은 관측된 결과를 다음 입력에 반영하고, **피드포워드**(feedforward)는 목표와 모델 등을 이용해 필요한 입력을 미리 구성한다. 두 방법을 함께 사용할 수 있다. 목표가 위치인지, 속도인지, 힘인지에 따라 비교하는 값과 하위 명령이 달라진다. 예를 들어 물체 무게의 모델로 중력을 버티는 입력을 미리 더하고, 실제 관절각과 목표각의 차이를 피드백으로 줄일 수 있다. 위치 제어(position control)·힘 제어(force control)·상호작용 제어(interaction control)는 연결된 기능이지만 동의어는 아니다. [제어 개요](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-1-control-system-overview/)

<a id="setpoint"></a>

### 목표가 그대로여도 제어는 반복된다

목표 각도 60°에 도착한 관절을 그대로 유지하려면 목표 각도와 실제 각도, 목표 속도 0과 실제 속도를 계속 비교해야 한다. 외부에서 밀리거나 하중이 바뀌면 다시 보정할 수 있어야 하기 때문이다. 이런 **고정 목표값 제어**(setpoint control)는 새 과업이 없는 대기 중에도 유효하다. [오버뷰의 대기 중 제어](../robotics_system_overview.md#closed-loop)

**비례·미분 제어**(proportional-derivative control, PD)는 위치 오차와 속도 오차를 함께 사용하는 방법이다. 정지한 목표로 이동할 때 위치 오차에 따른 항은 목표로 되돌리려는 역할을 하고, 속도에 따른 항은 움직임을 가라앉히는 **감쇠**(damping) 역할을 할 수 있다. 감쇠가 부족하면 목표를 지나쳤다가 되돌아오는 진동이 생길 수 있다. 반대로 오차를 줄이려는 반응을 무조건 크게 해도 구동 한계·지연·잡음 때문에 문제가 생길 수 있다. [PD 제어와 적용 조건](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-4-motion-control-with-torque-or-force-inputs-part-1-of-3/)

<a id="interaction"></a>

### 접촉할 때는 무엇을 목표로 삼는가?

손이 빈 공간을 이동할 때에는 위치가 중요하지만 물체에 닿은 뒤에는 얼마나 세게 누르는지도 중요해진다. **힘 제어**(force control)는 환경에 가하는 힘·토크를 목표에 맞추려는 제어다. 예를 들어 접촉 후 목표 힘이 5 N이고 측정값이 3 N이라면 힘 오차는 2 N이다. 이 차이를 줄이도록 입력을 조정하는 구성을 생각할 수 있다. 수치는 원리 설명용이다.

이때 5 N을 관절 모터에 그대로 명령하는 것은 아니다. 말단의 힘과 관절 토크 사이의 기구학적 관계, 몸의 무게와 구동 조건을 사용해야 한다. 힘 센서의 측정값으로 보정하는 구성도 있고 모델로 필요한 입력을 구하는 구성도 있다. [힘 목표·관절 토크·센서 피드백의 관계](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-5-force-control/)

접촉 중 외력에 어떻게 반응할지를 정하는 방법도 있다. 아래 예들은 입력과 목표의 관계를 이해하기 위한 것으로, 실제 제어 구성은 구동 방식과 센싱에 따라 달라진다.

| 방법 | 정하려는 관계 | 직관적인 예 |
|---|---|---|
| **임피던스 제어**(impedance control) | 기준 움직임에서 벗어난 정도와 속도 등에 대해 어떤 힘으로 반응할지 정한다. | 손이 밀렸을 때 스프링처럼 되미는 힘과 흔들림을 가라앉히는 반응을 구성한다. |
| **어드미턴스 제어**(admittance control) | 측정한 외력에 대해 목표 위치·속도 등 움직임이 어떻게 바뀔지 정한다. | 손에 외력이 가해지면 목표 움직임을 바꾸어 손이 물러나게 한다. |
| **혼합 운동·힘 제어**(hybrid motion–force control) | 방향에 따라 운동 목표와 힘 목표를 나누어 다룬다. | 평평한 면을 닦으면서 면을 따라 이동하고, 면에 수직인 방향으로는 누르는 힘을 조절한다. |

스프링에 더해 가상 질량과 감쇠의 관계를 구성할 수도 있다. 이 방법을 읽을 때에는 **어떤 값을 관측하고, 어떤 움직임이나 힘을 목표로 만드는지** 연결하면 된다. [방향별 운동·힘 제어 예](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-6-hybrid-motion-force-control/) · [임피던스·어드미턴스의 교재 설명](https://hades.mech.northwestern.edu/images/b/b2/MR-2up.pdf)
