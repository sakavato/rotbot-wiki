# 근거와 보충 자료

[처음으로](README.md)

본문의 기술 설명에 사용한 근거와 선택 심화 자료를 정리했다. 각 자료의 참고 위치와 적용 범위를 함께 확인할 수 있다. 전체 연결도는 기능 간 관계를 설명하는 학습용 도식이며, 특정 제품의 구현을 나타내지는 않는다.

## 1. 기초 개념의 기술적 근거

| 자료 | 사용한 부분 | 범위 |
|---|---|---|
| OpenStax, [Newton’s Second Law](https://openstax.org/books/university-physics-volume-1/pages/5-3-newtons-second-law), [Torque](https://openstax.org/books/university-physics-volume-1/pages/10-6-torque) | 알짜힘·질량·가속도, 토크와 힘의 작용 거리 | 본문의 1 kg·거리·토크 예는 설명용 계산이며, 정지한 평면 모델에서 팔 자체의 무게를 제외함 |
| Lynch·Park, [Modern Robotics 3.3.1](https://modernrobotics.northwestern.edu/nu-gm-book-resource/3-3-1-homogeneous-transformation-matrices/) | 좌표계 사이의 위치·방향 변환 | 20 cm + 30 cm 예는 축이 나란한 경우의 설명용 가정 |
| Lynch·Park, [Modern Robotics 2.5](https://modernrobotics.northwestern.edu/nu-gm-book-resource/2-5-task-space-and-workspace/) | Task space와 workspace | 용어 구분 |
| 같은 교재, [Inverse Kinematics](https://modernrobotics.northwestern.edu/nu-gm-book-resource/inverse-kinematics-of-open-chains/) | FK와 IK의 관계, 해의 존재·다중성 | 개념 설명 |
| 같은 교재, [5.1.1 Jacobian](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-1-1-space-jacobian/), [5.3 Singularities](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-3-singularities/) | 속도·힘 관계, 일자로 편 평면 팔의 순간 운동과 특이점 | 설명용 팔 자세의 예; 공식의 상세 유도는 초안 밖 |
| 같은 교재, [8.9 Actuation, Gearing, Friction](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-9-actuation-gearing-and-friction/) | 모터 구조, 감속비·속도·토크·기계적 출력, 마찰·관성과 토크 제어 조건 | 특정 제조사의 성능 판단에는 사용하지 않음 |
| 같은 교재, [8.3 Inverse dynamics](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-3-newton-euler-inverse-dynamics/) | 운동과 필요한 힘·토크의 관계 | 고정 기반 개방 사슬 설명을 휴머노이드 전체에 그대로 적용하지 않음 |
| 같은 교재, [9.4 Time scaling](https://modernrobotics.northwestern.edu/nu-gm-book-resource/9-4-time-optimal-time-scaling-part-1-of-3/), [10.1 Motion planning](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-1-overview-of-motion-planning/) | 경로·시간·실행 가능성 | 개별 알고리즘 우열을 결정하지 않음 |
| 같은 교재, [11.1 Control](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-1-control-system-overview/), [11.5 Force control](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-5-force-control/) | 폐루프와 구동, 드라이버 내부 전류·토크 피드백, 힘 목표·관절 토크 | 5 N 목표와 3 N 측정 예는 설명용이며 제어기 설정값이 아님 |
| 같은 교재, [11.4 Motion Control with Torque or Force Inputs](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-4-motion-control-with-torque-or-force-inputs-part-1-of-3/) | 고정 목표값 유지, 위치·속도 오차, PD 제어, 구동 한계·모델 오차의 영향 | 실제 제어기 설정값이나 안정성 보장으로 사용하지 않음 |
| Tedrake, [Manipulator Control](https://manipulation.mit.edu/force.html) | 중력 보상, 접촉 시 위치·힘·임피던스 제어의 차이 | 직관과 입력·출력 관계에 사용; 특정 로봇의 제어 구조를 가정하지 않음 |
| Lynch·Park, Modern Robotics [11.6 Hybrid Motion–Force Control](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-6-hybrid-motion-force-control/) | 면을 따라 움직이며 수직 방향으로 누르는 예 | 운동·힘 목표를 방향에 따라 나누는 직관 |
| Lynch·Park, Modern Robotics [2017년 5월 사전 공개본 PDF](https://hades.mech.northwestern.edu/images/b/b2/MR-2up.pdf) | §11.6–11.7, 인쇄 쪽 433–445: hybrid·impedance·admittance | 입력·출력 관계 설명에 사용; 전체 수식 재현 없음 |
| 같은 교재, [12.2.1 Friction](https://modernrobotics.northwestern.edu/nu-gm-book-resource/12-2-1-friction/), [12.2.3 Force closure](https://modernrobotics.northwestern.edu/nu-gm-book-resource/12-2-3-force-closure/) | 접촉의 모델 조건·마찰 원뿔과 파지 | 마찰계수 0.5·수직항력 10 N은 설명용 가정; 실제 구동력·파지 성공 보장과 구분 |
| Tedrake, [Highly-articulated Legged Robots](https://underactuated.mit.edu/humanoids.html) | 접촉력·CoM·CoP·ZMP, 단순화의 한계 | 갱신되는 강의노트; 미완성 WBC 절을 단독 근거로 사용하지 않음 |
| Tedrake, [Planning and Control through Contact](https://underactuated.mit.edu/contact.html) | 접촉 모드와 부유 기저: Deriving hybrid models 절의 몸체 6자유도·접촉 제약 | 몸체의 배치가 세계에 고정되지 않는다는 설명이며 실제로 공중에 떠 있다는 의미가 아님 |
| Tedrake, [Trajectory Optimization](https://underactuated.mit.edu/trajopt.html) | 최적화·direct collocation·MPC | 문제 형식과 실행 방식의 구분 |
| Texas Instruments, [Real-Time Control Reference Guide](https://www.ti.com/lit/pdf/slyy211#page=67) | 2021년판 인쇄 67쪽의 PWM과 듀티비 정의 | 전기 입력을 만드는 방식의 개념 설명. 특정 모터 드라이버의 설정값·성능을 제시하지 않음 |
| Texas Instruments, [Motor Types 강의](https://www.ti.com/video/6067548423001)와 [강의 자료](https://www.ti.com/content/dam/videos/external-videos/en-us/4/3816841626001/6067548423001.mp4/subassets/precision_labs_motor_types.pdf), [Servo & stepper drives](https://www.ti.com/applications/industrial/industrial-automation/servo-stepper-drives/overview.html) | 브러시 DC·BLDC·스테퍼의 구동 차이, 서보의 피드백 구성 | 모터 종류와 제어 구성의 구분; 제품 선택·성능 비교는 하지 않음 |
| Modern Robotics [10.5 PRM](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-5-sampling-methods-for-motion-planning-part-1-of-2/)·[RRT](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-5-sampling-methods-for-motion-planning-part-2-of-2/), [10.7 Nonlinear Optimization](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-7-nonlinear-optimization/) | 샘플링·그래프 탐색의 결합, 최적화의 비용·제약·국소 해 | 각 접근의 역할을 설명하며, 고정된 실행 순서로 제시하지 않음 |

## 2. 연구 사례와 센싱

| 자료 | 사용한 부분 | 범위 |
|---|---|---|
| Cadena 외, [SLAM survey](https://arxiv.org/abs/1606.05830) | §II의 front-end/back-end·데이터 연관·추정 방식 | 2016년 개관; 벽 무늬의 대응 예는 설명용이며 최신 알고리즘 순위가 아님 |
| Hartley 외, [Contact-aided InEKF](https://arxiv.org/abs/1904.09251) | 초록에 설명된 IMU·기구학·접촉 기반 상태 추정 | 특정 이족 연구 사례; 전체 휴머노이드에 대한 성능 보장 없음 |
| Bewley 외, [SORT](https://arxiv.org/html/1602.00763) | §3.1–3.3의 검출·상태 예측과 갱신·데이터 연관, 선택 심화로 연결한 §4의 평가 | 상자 두 개의 예는 역할을 설명하기 위한 가정. 논문의 실제 평가는 보행자 추적이며 로봇 집기 성능을 검증한 것은 아님 |
| Kuindersma 외, [Atlas 통합 연구](https://dspace.mit.edu/entities/publication/6cb6cad7-9288-419c-aa5a-cc44744f2e91) | 인지·계획·추정·제어 통합의 연구 사례 | 2016년 논문; 현대 모든 휴머노이드의 표준 구조라는 의미가 아님 |
| [Benchmarking Whole-Body Controllers on the TALOS Humanoid Robot](https://www.frontiersin.org/journals/robotics-and-ai/articles/10.3389/frobt.2022.826491/full) | §1.2·3.1·3.2의 동시 목표, 엄격한 우선순위와 가중치·제약의 구분 | 2022년 시뮬레이션 비교 연구; 본문의 집기·대기 상황은 원리를 연결한 설명용 예이며 논문의 실험 결과가 아님 |
| [Humanoid-Gym](https://arxiv.org/abs/2404.05695) | 2024년 논문 초록의 학습 기반 보행·sim-to-real 사례 | 적용 사례로만 사용; 상세 재현·비교 실험 미수행 |
| [ASAP](https://arxiv.org/abs/2502.01143) | 2025년 논문 초록의 시뮬레이션·실제 동역학 차이 | 최신 기술 전수 조사나 일반 성능 보장이 아님 |
| JCGM, VIM3 [정확도 §2.13](https://jcgm.bipm.org/vim/en/2.13.html)·[정밀도 §2.15](https://jcgm.bipm.org/vim/en/2.15.html)·[반복성 §2.21](https://jcgm.bipm.org/vim/en/2.21.html)과 [반복 조건 §2.20](https://jcgm.bipm.org/vim/en/2.20.html) | 참값과의 가까움, 반복값의 퍼짐, 반복성을 평가하는 조건의 구분 | 10 N을 측정하는 A·B 센서의 수치는 설명용 가정이며 특정 제품의 시험 결과가 아님 |
| NIST, [Basic definitions of uncertainty](https://physics.nist.gov/cuu/Uncertainty/glossary.html) | 측정 불확실성과 값의 퍼짐, 표준편차·구간 표현 | 특정 센서의 수치 평가가 아닌 용어 설명 |
| ATI, [Force/Torque FAQ](https://www.ati-ia.com/library/documents/FT_FAQ.pdf) | §2 측정 범위·분해능·불확실성·포화 시 유효성 | 2020년 제조사 문서; 포화가 항상 같은 숫자를 출력한다거나 모든 센서의 상태 신호가 동일하다고 가정하지 않음 |
| Stanford CHARM Lab, [Research](https://charm.stanford.edu/Main/Research)·[Teleoperation and Haptics for Surgery](https://charm.stanford.edu/Main/TeleoperationAndHapticsForSurgery) | 피부 감각·운동 감각, 힘·피부 자극 피드백 | 컵과 원격 조작은 역할을 설명하는 예이며 연구 성능을 주장하지 않음 |
| [MEMS-Based Tactile Sensors: Materials, Processes and Applications in Robotics](https://pmc.ncbi.nlm.nih.gov/articles/PMC9782357/), Kistler [Piezo vs. strain gauge](https://www.kistler.com/US/en/piezo-vs.-strain-gauge/C00000145) | 촉각 배열, 저항·정전용량·광학 방식, 압전 전하와 장시간 측정 조건 | 변환 원리만 사용; 소재·제품의 우열이나 최신 동향을 판단하지 않음 |
| ATI, [Multi-Axis Force/Torque Sensors](https://ati-ia.com/products/ft/sensors.aspx)·[System Interfaces](https://www.ati-ia.com/products/ft/ft_SystemInterfaces.aspx) | 6축 힘·토크, 게이지 신호의 증폭·디지털화·보정 | 손끝과 손목은 측정 위치를 비교하는 설명용 예; 제품 사양 수치는 사용하지 않음 |
| JCGM [Step response time §4.23](https://jcgm.bipm.org/vim/en/4.23.html), NI [Using a Digitizer for Time-Domain Measurements](https://www.ni.com/en/shop/electronic-test-instrumentation/oscilloscopes/using-a-digitizer-for-time-domain-measurements.html) | 응답 시간, 대역폭과 샘플링 속도의 구분 | 동적 측정의 기본 구분; 특정 센서에 필요한 주파수를 정하지 않음 |
| Qin 외, [VINS-Mono](https://arxiv.org/html/1708.03852v1)·[Relocalization, Global Optimization and Map Merging](https://arxiv.org/abs/1803.01549) | 시각·관성 결합, VIO 누적 오차와 루프 폐합·지도 보정의 관계 | 센서 역할과 추정 범위 설명; 성능 수치를 일반화하지 않음 |
| VOT, [Tracker integration](https://votchallenge.net/howto/integration_multiobject.html) | 지정한 대상의 초기화와 단일·복수 추적 예 | SORT와 함께 추적 문제의 범위를 구분; 평가 순위·구현 방법은 설명하지 않음 |

## 3. SW 인터페이스와 적용 조건

| 자료 | 사용한 부분 | 범위 |
|---|---|---|
| ROS, [REP-105 저장소 원문](https://raw.githubusercontent.com/ros-infrastructure/rep/master/rep-0105.rst) | `map`·`odom`·`base_link`의 의미 | 개념 문서와 적용 사례에서 이 규약을 채택한 구성을 가정; 실제 로봇 채택 여부 미정 |
| ROS 2, [Real-time background](https://design.ros2.org/articles/realtime_background.html) | 시간 제약과 지연 | 보편적인 제어 주기 수치를 추정하지 않음 |
| Zephyr, [Scheduling](https://docs.zephyrproject.org/latest/kernel/services/scheduling/index.html) | 우선순위·선점·인터럽트와 스케줄링 | RTOS 동작의 구체적 예. 실제 로봇의 채택 여부나 모든 RTOS의 동일한 동작을 가정하지 않음 |
| ROS 2, [QoS design](https://design.ros2.org/articles/qos.html) | 신뢰성·이력·큐 정책 | 설계 개념만 사용; 배포판 기본 설정이나 버전별 API는 미검증 |
| ROS 2, [Actions design](https://design.ros2.org/articles/actions.html) | 목표·피드백·결과·취소 | 장시간 작업 인터페이스의 사례 |

ROS 문서 사이트 일부는 자동 접근 차단이 있어 공식 설계 문서와 저장소 원문을 사용했다. 연구 논문의 초록을 참고한 부분은 초록이 설명하는 기능·연구 범위를 넘어 성능 수치나 재현성을 주장하지 않았다.

## 4. 선택 심화 자료와 적용 범위

[선택 심화와 확장 방향](concepts.md#further-study)은 현재 개념에서 이어질 질문을 안내한다. 위 표의 자료 외에 다음 두 자료를 추가로 연결했다.

| 자료 | 이어서 알아볼 내용 | 범위 |
|---|---|---|
| Modern Robotics [5.4 Manipulability](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-4-manipulability/) | 자코비안·특이점에서 조작성 타원체와 지표로 확장 | 자코비안·특이점 다음에 읽을 선택 심화. 조작성 지표의 계산은 본문 범위 밖 |
| NIST [Combining uncertainty components](https://physics.nist.gov/cuu/Uncertainty/combination.html) | 측정 모델에서 입력들의 불확실성을 결과에 결합하는 방법 | 미분·공분산을 사용하는 계산은 선택 심화 |

특정 제품에 적용하려면 해당 모델·버전·시험 조건의 자료를 별도로 확인해야 한다.
