# 근거와 보충 자료

[처음으로](README.md)

2026-09-20 초안의 출처 목록에 2026-09-22까지 기초 설명과 유지 목표·전신 제어 설명을 보강하는 데 사용한 자료를 추가했다. 본문에는 해당 주장을 뒷받침하는 링크를 가까이 배치했다. 여기서는 어떤 목적으로 참고했는지와 해석의 범위를 기록한다. 전체 연결도는 아래 자료의 기능 관계를 학습 목적으로 종합한 작성자의 도식이며, 특정 제품의 아키텍처를 전사한 것이 아니다. 교재나 논문 전체의 검증을 완료했다는 뜻은 아니다.

## 1. 자료의 출발점

이 자료는 작성자가 청강한 5일 사내 교육의 키워드 메모와 로봇 제어 파이프라인 도식을 출발점으로 삼았다. 독자와 학습 목적은 [첫 페이지](README.md#audience)에, 메모의 주요 주제와 보완 관계는 [검토 기록](curriculum_coverage_and_gaps.md)에 요약했다.

원본 메모와 제작 논의는 배포본에 포함하지 않으며, 읽기 위해 해당 파일을 열 필요는 없다. 메모는 어떤 주제를 접했는지에 대한 근거이지 기술적 정확성의 보증이 아니다. 기술 설명은 아래 교재·논문·공식 문서를 별도 근거로 삼는다.

## 2. 기초 개념의 기술적 근거

| 자료 | 사용한 부분 | 범위 |
|---|---|---|
| OpenStax, [Newton’s Second Law](https://openstax.org/books/university-physics-volume-1/pages/5-3-newtons-second-law), [Torque](https://openstax.org/books/university-physics-volume-1/pages/10-6-torque) | 알짜힘·질량·가속도, 토크와 힘의 작용 거리 | 본문의 1 kg·거리·토크 예는 설명용 계산이며, 정지한 평면 모델에서 팔 자체의 무게를 제외함 |
| Lynch·Park, [Modern Robotics 3.3.1](https://modernrobotics.northwestern.edu/nu-gm-book-resource/3-3-1-homogeneous-transformation-matrices/) | 좌표계 사이의 위치·방향 변환 | 20 cm + 30 cm 예는 축이 나란한 경우의 설명용 가정 |
| Lynch·Park, [Modern Robotics 2.5](https://modernrobotics.northwestern.edu/nu-gm-book-resource/2-5-task-space-and-workspace/) | Task space와 workspace | 용어 구분 |
| 같은 교재, [Inverse Kinematics](https://modernrobotics.northwestern.edu/nu-gm-book-resource/inverse-kinematics-of-open-chains/) | FK와 IK의 관계, 해의 존재·다중성 | 개념 설명 |
| 같은 교재, [5.1.1 Jacobian](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-1-1-space-jacobian/), [5.3 Singularities](https://modernrobotics.northwestern.edu/nu-gm-book-resource/5-3-singularities/) | 속도·힘 관계, 특이점 | 공식의 상세 유도는 초안 밖 |
| 같은 교재, [8.9 Actuation, Gearing, Friction](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-9-actuation-gearing-and-friction/) | 구동·전달계 특성과 동역학 | 특정 제조사의 성능 판단에는 사용하지 않음 |
| 같은 교재, [8.3 Inverse dynamics](https://modernrobotics.northwestern.edu/nu-gm-book-resource/8-3-newton-euler-inverse-dynamics/) | 운동과 필요한 힘·토크의 관계 | 고정 기반 개방 사슬 설명을 휴머노이드 전체에 그대로 적용하지 않음 |
| 같은 교재, [9.4 Time scaling](https://modernrobotics.northwestern.edu/nu-gm-book-resource/9-4-time-optimal-time-scaling-part-1-of-3/), [10.1 Motion planning](https://modernrobotics.northwestern.edu/nu-gm-book-resource/10-1-overview-of-motion-planning/) | 경로·시간·실행 가능성 | 개별 알고리즘 우열을 결정하지 않음 |
| 같은 교재, [11.1 Control](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-1-control-system-overview/), [11.5 Force control](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-5-force-control/) | 폐루프와 구동, 힘 피드백 | 모든 제품의 제어 구조를 고정하지 않음 |
| 같은 교재, [11.4 Motion Control with Torque or Force Inputs](https://modernrobotics.northwestern.edu/nu-gm-book-resource/11-4-motion-control-with-torque-or-force-inputs-part-1-of-3/) | 고정 목표값 유지, 위치·속도 오차, PD 제어, 구동 한계·모델 오차의 영향 | 실제 제어기 설정값이나 안정성 보장으로 사용하지 않음 |
| Tedrake, [Manipulator Control](https://manipulation.mit.edu/force.html) | 중력 보상, 접촉 시 위치·힘·임피던스 제어의 차이 | 직관과 입력·출력 관계에 사용; 특정 로봇의 제어 구조를 가정하지 않음 |
| Lynch·Park, Modern Robotics [2017년 5월 사전 공개본 PDF](https://hades.mech.northwestern.edu/images/b/b2/MR-2up.pdf) | §11.6–11.7, 인쇄 쪽 433–445: hybrid·impedance·admittance | 입력·출력 관계 설명에 사용; 전체 수식 재현 없음 |
| 같은 교재, [12.2.1 Friction](https://modernrobotics.northwestern.edu/nu-gm-book-resource/12-2-1-friction/), [12.2.3 Force closure](https://modernrobotics.northwestern.edu/nu-gm-book-resource/12-2-3-force-closure/) | 접촉의 모델 조건과 파지 | 실제 구동력·파지 성공 보장과 구분 |
| Tedrake, [Highly-articulated Legged Robots](https://underactuated.mit.edu/humanoids.html) | 접촉력·CoM·CoP·ZMP, 단순화의 한계 | 갱신되는 강의노트; 미완성 WBC 절을 단독 근거로 사용하지 않음 |
| Tedrake, [Planning and Control through Contact](https://underactuated.mit.edu/contact.html) | 접촉 모드·hybrid dynamics·floating base | 구체적인 제품의 접촉 판정법은 별도 |
| Tedrake, [Trajectory Optimization](https://underactuated.mit.edu/trajopt.html) | 최적화·direct collocation·MPC | 문제 형식과 실행 방식의 구분 |

## 3. 연구 사례와 센싱

| 자료 | 사용한 부분 | 범위 |
|---|---|---|
| Cadena 외, [SLAM survey](https://arxiv.org/abs/1606.05830) | SLAM 문제와 front-end/back-end·추정 방식 | 2016년 개관; 최신 알고리즘 순위가 아님 |
| Hartley 외, [Contact-aided InEKF](https://arxiv.org/abs/1904.09251) | 초록에 설명된 IMU·기구학·접촉 기반 상태 추정 | 특정 이족 연구 사례; 전체 휴머노이드에 대한 성능 보장 없음 |
| Bewley 외, [SORT](https://arxiv.org/abs/1602.00763) | 초록의 검출·연관·필터 기반 물체 추적 구조 | 로봇 자체의 위치 추정과 구분하는 사례 |
| Kuindersma 외, [Atlas 통합 연구](https://dspace.mit.edu/entities/publication/6cb6cad7-9288-419c-aa5a-cc44744f2e91) | 인지·계획·추정·제어 통합의 연구 사례 | 2016년 논문; 현대 모든 휴머노이드의 표준 구조라는 의미가 아님 |
| [Benchmarking Whole-Body Controllers on the TALOS Humanoid Robot](https://www.frontiersin.org/journals/robotics-and-ai/articles/10.3389/frobt.2022.826491/full) | §1.2·3.1·3.2의 동시 목표, 엄격한 우선순위와 가중치·제약의 구분 | 2022년 시뮬레이션 비교 연구; 본문의 집기·대기 상황은 원리를 연결한 설명용 예이며 논문의 실험 결과가 아님 |
| [Humanoid-Gym](https://arxiv.org/abs/2404.05695) | 2024년 논문 초록의 학습 기반 보행·sim-to-real 사례 | 적용 사례로만 사용; 상세 재현·비교 실험 미수행 |
| [ASAP](https://arxiv.org/abs/2502.01143) | 2025년 논문 초록의 시뮬레이션·실제 동역학 차이 | 최신 기술 전수 조사나 일반 성능 보장이 아님 |
| NIST, [Basic definitions of uncertainty](https://physics.nist.gov/cuu/Uncertainty/glossary.html) | 측정 불확실성과 값의 퍼짐, 표준편차·구간 표현 | 특정 센서의 수치 평가가 아닌 용어 설명 |
| ATI, [Force/Torque FAQ](https://www.ati-ia.com/library/documents/FT_FAQ.pdf) | §2 측정 범위·분해능·불확실성·포화 | 2020년 제조사 문서; 모든 센서의 상태 신호가 동일하다고 가정하지 않음 |

## 4. SW 인터페이스와 적용 조건

| 자료 | 사용한 부분 | 범위 |
|---|---|---|
| ROS, [REP-105 저장소 원문](https://raw.githubusercontent.com/ros-infrastructure/rep/master/rep-0105.rst) | `map`·`odom`·`base_link`의 의미 | 이 규약을 채택한 구성의 예. 실제 로봇 채택 여부 미정 |
| ROS 2, [Real-time background](https://design.ros2.org/articles/realtime_background.html) | 시간 제약과 지연 | 보편적인 제어 주기 수치를 추정하지 않음 |
| Zephyr, [Scheduling](https://docs.zephyrproject.org/latest/kernel/services/scheduling/index.html) | 우선순위·선점·인터럽트와 스케줄링 | RTOS 동작의 구체적 예. 실제 로봇의 채택 여부나 모든 RTOS의 동일한 동작을 가정하지 않음 |
| ROS 2, [QoS design](https://design.ros2.org/articles/qos.html) | 신뢰성·이력·큐 정책 | 설계 개념만 사용; 배포판 기본 설정이나 버전별 API는 미검증 |
| ROS 2, [Actions design](https://design.ros2.org/articles/actions.html) | 목표·피드백·결과·취소 | 장시간 작업 인터페이스의 사례 |
| OSHA, [Technical Manual IV-4](https://www.osha.gov/otm/section-4-safety-hazards/chapter-4) | 로봇 특성과 적용 작업의 위험 평가 구분 | 미국 산업용 적용 설명. 국내 법규·휴머노이드 인증 적합성은 판단하지 않음 |

ROS 문서 사이트 일부는 자동 접근 차단이 있어 공식 설계 문서와 저장소 원문을 사용했다. 연구 논문의 초록을 참고한 부분은 초록이 설명하는 기능·연구 범위를 넘어 성능 수치나 재현성을 주장하지 않았다.

## 5. 미확인 내용을 남기는 방법

CI-TrajOpt의 정확한 지칭, 일부 원본 오기, 특정 제조사의 제어 방식, 개별 센서·모터의 성능은 아직 확정하지 않았다. [교육 메모 검토](curriculum_coverage_and_gaps.md#corrections)에서 위치를 추적할 수 있다.

공개 후 보충할 때는 대상 문장이나 관계, 성립 조건, 근거의 절·버전, 기존 설명에서 달라지는 부분을 함께 남기는 방식을 제안한다. 게시 플랫폼이 바뀌어도 이 정보는 편집 가능한 원고와 함께 유지한다.
