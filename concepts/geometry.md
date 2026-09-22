# 기구학과 동역학

[개념 찾아보기](../concepts.md) · [오버뷰](../robotics_system_overview.md) · [일자별 안내](../learning_path.md)

관절의 움직임을 손의 위치와 연결하고, 그 움직임에 필요한 힘을 구분한다.

[몸의 표현](#geometry) · [동역학](#dynamics)

<a id="geometry"></a>
## 몸의 구조와 움직임을 표현하는 언어

**링크**(link)는 몸체의 부분, **관절**(joint)은 그 사이의 연결이다. **자유도**(degrees of freedom, DOF)는 독립적으로 변할 수 있는 구성의 차원을 말한다. **말단 장치**(end effector)는 손·그리퍼 등 작업에 직접 사용하는 말단이다. 작업을 정의하는 변수와 로봇의 모든 관절 변수가 같지는 않다. 예를 들어 평면에서 두 회전 관절로 움직이는 팔은 두 관절각으로 구성을 표현한다. 손을 어디에 놓을지는 평면의 위치 두 값으로 표현할 수 있지만, 그 위치에서 손의 방향까지 임의로 정할 수 있는지는 별도 문제다.

| 키워드 | 의미 | 연결해서 읽을 것 |
|---|---|---|
| 구성(configuration) / 구성 공간(configuration space, C-space) | 몸의 배치를 나타내는 변수의 조합 / 그 조합들이 이루는 공간 | 몸 전체의 충돌 검사, 관절 한계, [몸체의 위치·방향](contact.md#floating-base) |
| 관절 공간(joint space) | 관절 변수로 표현한 공간 | 관절 상태·명령과 기구학 |
| 작업 공간(task space) | 수행하려는 작업을 자연스럽게 표현하는 공간 | 손의 위치만 요구하는지, 방향까지 요구하는지 |
| 도달 가능 작업 영역(workspace) | 로봇 말단이 도달할 수 있는 위치·방향의 범위 | 링크 구조·관절 범위와 작업 요구 |
| 위치·방향을 합친 배치(pose) | 물체나 손이 어디에 있고 어느 쪽을 향하는지 함께 나타낸 것 | 같은 위치에서도 손을 위에서 내릴지 옆으로 넣을지 구분 |

예를 들어 바닥에 고정된 평면의 두 관절 팔에서 `q = [30°, 60°]`는 두 관절각으로 표현한 구성 한 개다. 같은 손의 위치는 기준 좌표계의 `(x, y)`로 표현할 수 있다. **관절각은 몸의 배치**, **손 좌표는 작업에서 관심 있는 위치**를 나타낸다. 손의 목표가 정해졌어도 팔꿈치가 선반에 부딪힐 수 있으므로 몸 전체의 구성을 함께 확인해야 한다.

Task space와 workspace는 모두 ‘작업 공간’으로 번역되기도 한다. 둘을 같은 뜻으로 쓰면 “작업에서 원하는 것”과 “로봇이 도달할 수 있는 것”을 혼동한다. [Modern Robotics 2.5](https://modernrobotics.northwestern.edu/nu-gm-book-resource/2-5-task-space-and-workspace/)

**정기구학**(forward kinematics, FK)은 주어진 관절 구성에서 말단의 위치·방향을 구한다. **역기구학**(inverse kinematics, IK)은 목표 말단 배치를 만족하는 관절 구성을 찾는다. IK의 해는 없거나 여럿일 수 있다. 해를 하나 얻었다고 그 자세까지 충돌 없이 이동하는 경로까지 얻은 것은 아니다. [역기구학](https://modernrobotics.northwestern.edu/nu-gm-book-resource/inverse-kinematics-of-open-chains/)

### 지금 자세에서 어느 방향으로 움직일 수 있는가?

**자코비안**(Jacobian)은 현재 구성에서 관절 속도를 손의 선속도(linear velocity)·각속도(angular velocity)와 연결한다. 같은 속도로 어깨를 돌려도 팔을 접었을 때와 길게 뻗었을 때 손의 움직임이 달라진다. 따라서 이 관계는 현재 자세에 따라 바뀐다. 제어에서는 손을 원하는 방향·속도로 움직이기 위한 관절 속도를 구할 때 사용하며, 손의 힘과 관절 토크를 연결할 때도 등장한다. [Jacobian](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-1-1-space-jacobian/)

**특이점**(singularity)은 만들어낼 수 있는 손의 독립적인 순간 운동 방향이 평소보다 줄어드는 구성이다. 평면의 두 관절 팔을 일자로 완전히 뻗으면, 그 순간 각 관절의 회전이 만드는 손의 속도는 팔에 직각인 방향뿐이다. 팔 길이 방향의 속도를 바로 만들 수 없고, 굽혀서 자세를 바꾸어야 한다. 이는 안쪽 위치에 나중에 도달할 수 있는지와 별개의 문제다. **도달 가능한 위치**와 **현재 자세에서 만들 수 있는 순간 움직임**을 구분하는 이유다. [펴진 팔의 특이점 예](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-3-singularities/)

<a id="dynamics"></a>
### 기구학에서 동역학으로

힘·토크와 관성의 출발점은 [본문의 물체 들기 예](../robotics_system_overview.md#vertical)다. 직선 운동에서는 물체에 작용하는 힘들을 합친 **알짜힘**(net force)과 가속도 사이에 `F = m × a`의 관계가 있다. `F`는 힘[N], `m`은 질량[kg], `a`는 가속도[m/s²]다. 같은 질량을 더 크게 가속하려면 더 큰 알짜힘이 필요하다. 물체를 정지 상태로 들고 있다면 가속도와 알짜힘은 0이지만, 손이 주는 위쪽 힘과 중력이 각각 0인 것은 아니다. 두 힘이 균형을 이루는 것이다. [뉴턴의 운동 법칙](https://openstax.org/books/university-physics-volume-1/pages/5-3-newtons-second-law)

**기구학**(kinematics)이 구성과 운동의 기하학적 관계를 다룬다면, **동역학**(dynamics)은 그 운동과 힘·토크의 관계를 다룬다. **역동역학**(inverse dynamics)은 주어진 운동과 외력 조건에서 필요한 관절 힘·토크를 구한다. **정동역학**(forward dynamics)은 주어진 힘·토크에서 운동의 변화를 구하며 시뮬레이션에 연결된다. [역동역학과 정동역학의 관계](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-3-newton-euler-inverse-dynamics/)

예를 들어 손이 같은 경로를 지나더라도 하중·자세·가속도가 달라지면 필요한 입력을 다시 검토해야 한다. 그래서 기구학적으로 가능한 자세, 동역학적으로 실행 가능한 움직임, 하드웨어가 실제로 만들 수 있는 출력은 구분해서 본다. 휴머노이드에서는 여기에 [몸체와 접촉 조건](contact.md#contact)이 더해진다.
