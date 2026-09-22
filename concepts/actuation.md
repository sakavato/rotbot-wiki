# 모터와 구동

[개념 찾아보기](../concepts.md) · [오버뷰](../robotics_system_overview.md) · [일자별 안내](../learning_path.md)

전기 입력이 관절의 힘과 움직임으로 이어지는 과정과, 구동계가 정하는 한계를 살펴본다.

[모터 원리](#motor-principle) · [종류·서보](#motor-types) · [감속비](#gearing) · [구동 조건](#hardware) · [PWM](#motor-control)

<a id="motor-principle"></a>
## 전류가 회전력으로 바뀌는 과정

**전기 모터**(electric motor)는 전기 에너지를 기계적 일로 바꾸는 장치다. 여기서 다루는 모터는 코일에 흐르는 전류와 자기장의 상호작용으로 **토크**(torque, 회전시키는 효과)를 만든다. 고정된 부분은 **고정자**(stator), 상대적으로 회전하는 부분은 **회전자**(rotor)다. 계속 회전시키려면 회전자 위치에 맞춰 코일에 전류를 흘리는 방향·순서를 바꿔야 한다. 이 전환을 **정류**(commutation)라고 한다. [모터 구조와 토크 발생](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-9-actuation-gearing-and-friction/)

팔을 들기 위한 목표 관절각이 제어기에 전달되면, 제어기와 구동부는 필요한 토크를 만들 전류를 조절한다. 따라서 **목표 관절각 → 제어 입력 → 모터 전류 → 토크**의 관계를 알아야 상위 명령과 실제 동작을 연결할 수 있다. 구체적인 피드백 구성은 [제어 문서](control.md#control)에서 이어 본다.

<a id="motor-types"></a>
## 모터 종류와 서보는 무엇이 다른가?

아래는 교육에서 다룬 대표적인 세 종류다. 같은 회전 운동을 만들더라도 전류를 전환하는 구조와 구동 방식이 다르다. [TI 모터 종류 강의와 자료](https://www.ti.com/video/6067548423001)

| 종류 | 기본 작동 | 함께 알아둘 점 |
|---|---|---|
| **브러시 DC 모터**(brushed DC motor) | 브러시와 정류자의 기계적 접촉으로 회전에 맞춰 전류 방향을 바꾼다. | 구동 구성이 비교적 단순하지만 브러시의 마모가 있다. |
| **브러시리스 DC 모터**(brushless DC motor, BLDC) | 전자 회로가 회전자 위치에 맞춰 코일 전류를 전환한다. | 기계식 브러시 대신 위치 정보와 전자적 정류가 필요하다. |
| **스테퍼 모터**(stepper motor) | 코일에 순서대로 전류를 흘려 회전자의 목표 위치를 단계적으로 바꾼다. | 스텝 명령 수로 이동량을 정할 수 있지만, 부하 등으로 스텝을 놓치면 실제 위치가 명령과 달라질 수 있다. |

**서보**(servo)는 실제 위치·속도 등을 피드백해 목표에 맞추는 구성을 가리킨다. 예를 들어 엔코더로 각도를 읽고 목표와 비교하면서 모터 입력을 조절한다. 따라서 ‘BLDC’는 모터의 종류를, ‘서보’는 피드백을 이용하는 구성을 설명하며, **BLDC 모터로 서보 구동계를 만들 수 있다.** 모터·센서·드라이버가 한 몸체에 들어가는지는 제품마다 다르다. [서보 구동과 위치 피드백](https://www.ti.com/applications/industrial/industrial-automation/servo-stepper-drives/overview.html)

<a id="gearing"></a>
## 감속하면 왜 더 큰 토크를 얻는가?

**감속비**(gear ratio)를 모터의 회전 속도와 출력축 속도의 비로 두자. 감속비가 10이면 모터가 10회전할 때 출력축은 1회전한다. 손실이 없는 이상적인 감속기라면 출력 속도는 1/10, 출력 토크는 10배가 된다. 예를 들어 모터 토크가 0.1 N·m라면 출력은 1 N·m다. 실제로는 손실 때문에 토크가 이보다 작다.

에너지를 새로 만들어내는 것은 아니다. 회전의 **기계적 출력**(mechanical power)은 `P = τ × ω`로, 토크 `τ`[N·m]와 각속도 `ω`[rad/s]의 곱이며 단위는 W다. 이상적인 감속기는 속도를 낮춘 만큼 토크를 높여 출력을 보존한다. 관절을 빨리 움직이는 요구와 무거운 팔을 버티는 요구를 함께 봐야 하는 이유다. [감속비·속도·토크·출력의 관계](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-9-actuation-gearing-and-friction/)

<a id="hardware"></a>
## 전달계와 센싱이 제어의 조건을 만든다

**구동기**(actuator)는 물리적 힘·움직임을 만들고, **전달계**(transmission)는 이를 관절에 전달한다. 감속기(speed reducer)는 속도와 토크를 바꾸며 마찰(friction)·관성(inertia) 등 동적 특성에도 영향을 준다. **역구동성**(backdrivability)은 외부 힘으로 구동계를 역으로 움직이기 쉬운 성질이다. 센서가 외력을 알아내는 것, 제어가 그 힘에 반응해 몸을 움직이는 것, 기계적으로 잘 역구동되는 것은 구분한다. [구동·감속·마찰](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-9-actuation-gearing-and-friction/)

모터 전류(motor current), 출력 관절 토크, 손끝의 힘은 서로 다른 물리량이다. 그 사이를 연결하려면 전달계·마찰·기구 모델이 필요하다. 예를 들어 손끝에서 같은 힘을 버티더라도 팔을 접었는지 뻗었는지에 따라 관절에 필요한 토크가 달라진다. 그래서 전류 하나만 보고 물체에 가한 힘이나 파지 성공을 바로 판단할 수 없다. [손의 힘과 관절 토크](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-5-force-control/)

관절 토크를 조절할 때는 토크 센서의 피드백을 사용할 수도 있고, 모터 전류와 전달계 모델로 필요한 입력을 구할 수도 있다. 감속비뿐 아니라 센서 위치, 마찰, 사용 가능한 명령 방식이 제어 구성을 결정한다. 그래서 감속비가 크다는 사실만으로 토크 제어 가능 여부를 단정하지 않는다. [관절 토크·전류 피드백의 구성](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-1-control-system-overview/) · [감속과 마찰의 영향](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-9-actuation-gearing-and-friction/)

<a id="motor-control"></a>
### 전기 입력을 만드는 방식과 제어 목표

**펄스 폭 변조**(pulse-width modulation, PWM)는 신호가 켜져 있는 시간의 비율을 바꾸는 방식이다. 모터 드라이버에서는 전력 스위칭에 이를 사용해 모터에 가하는 전기 입력을 조절할 수 있다. 한 주기 중 켜진 시간의 비율을 **듀티비**(duty cycle)라고 한다. [PWM과 듀티비, TI 참고서 67쪽](https://www.ti.com/lit/pdf/slyy211#page=67)

**속도 제어**(velocity control)의 목표는 모터나 관절의 속도를 원하는 값에 맞추는 것이다. 예를 들어 목표 속도와 측정 속도의 차이로 필요한 전류를 정하고, 드라이버 내부에서는 전류 피드백과 PWM으로 전기 입력을 조절하는 구성을 생각할 수 있다. 이때 속도는 제어할 물리량이고, PWM은 입력을 만들어내는 수단이다. 전류·속도·위치 중 무엇을 명령하고 어떤 값을 측정하는지에 따라 제어 구성이 달라진다. [제어기·드라이버·내부 피드백의 관계](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-1-control-system-overview/)
