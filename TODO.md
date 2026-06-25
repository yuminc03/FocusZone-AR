# FocusZone 개발 태스크 로드맵 (TODO.md)
본 문서는 실시간 3D 공간 소음 지도 제작 및 시각화 앱인 `FocusZone`의 개발을 단계별로 나누어 정리한 작업 목록입니다.

## Phase 1: 오디오 엔진 및 FFT 분석 모듈 구축 (Audio Analysis)
- [ ] 마이크 사용 권한 요청 로직 구현
AVFoundation 오디오 세션 설정 및 사용자 권한 동의 플로우 설계
- [ ] 실시간 오디오 버퍼 캡처 모듈 개발
마이크 입력을 실시간 PCM Buffer로 추출하는 오디오 캡처 엔진 생성
- [ ] FFT 분석 알고리즘 구현
Accelerate 프레임워크의 vDSP 패키지를 이용하여 고속 푸리에 변환 연산 처리
- [ ] 데시벨(dB) 측정 및 변환 로직 구현
특정 주파수 대역 및 전체 소음 수준을 dB 수치로 산출하는 로직 연동
- [ ] Swift Testing 기반 단위 테스트 작성
`@Test` 매크로를 사용하여 다양한 음역대 입력값에 대한 데시벨 산출 데이터 정확도 및 비동기 처리 검증

## Phase 2: LiDAR 기반 공간 인식 및 기본 메쉬 렌더링 (AR & Scene Reconstruction)
- [ ] ARKit Scene Reconstruction 초기 설정
ARWorldTrackingConfiguration 및 sceneReconstruction 활성화 처리
- [ ] ARView 컴포넌트 셋업
RealityKit의 ARView를 SwiftUI 컨테이너 구조(`UIViewRepresentable`)와 통합
- [ ] 실시간 3D 메쉬 생성 기능 연동
LiDAR 스캐너가 인식한 방의 구조(벽면, 가구 등)를 ARMeshAnchor 형태로 수집
- [ ] 더미 히트맵 셰이더 적용
스캔된 메쉬 표면에 실시간 소음 수치와 매핑하기 전, 단일 파란색(조용함 기준) 머티리얼을 씌워 시각화 여부 테스트

## Phase 3: 실시간 히트맵 보간 및 소리 반사 벡터 연산 (Integration & Math)
- [ ] 3D 소음 좌표 수집 및 데이터 버퍼링
iPhone의 실시간 3D 카메라 위치(Camera Transform)와 측정된 데시벨 값을 일치시켜 메모리 버퍼에 캐싱
- [ ] 히트맵 색상 보간(Color Interpolation) 알고리즘 구현
인접한 노드들의 데시벨 세기를 거리 기반 가중치로 계산하여 파란색(안정) ~ 빨간색(경고) 스펙트럼으로 메쉬 컬러 보간
- [ ] 가상 소음원 설정 및 기하학적 정보 추출
시뮬레이션할 소음원의 3D 위치를 정의하고 충돌 메쉬의 법선 벡터(Normal Vector) 정보 추출
- [ ] 소리 반사 벡터 연산 로직 구현
선형대수학 입사각/반사각 공식을 적용하여 반사 벡터($\mathbf{r} = \mathbf{d} - 2(\mathbf{d} \cdot \mathbf{n})\mathbf{n}$) 연산
- [ ] 반사 경로 3D 선형 그래픽(Ray-tracing Line) 가시화
RealityKit의 Entity 또는 Custom Geometry를 활용해 입사 및 반사 경로를 선으로 그리는 렌더링 모듈 추가

## Phase 4: UI/UX 고도화, 메모리 최적화 및 안정성 검증
- [ ] SwiftUI 오버레이 컨트롤 패널 개발
반투명 효과(Glassmorphism)와 다크 모드가 적용된 제어 화면 구성
- [ ] 온보딩 및 공간 스캐닝 안내 애니메이션 추가
사용자가 방을 둘러보도록 유도하는 네이티브 디자인 인터페이스 개발
- [ ] 메모리 라이프사이클 관리 및 메모리 누수 방지
동적 측정 데이터가 백그라운드 진입 또는 앱 종료 시 완전히 free되도록 휘발성 아키텍처 수명 주기 검증
- [ ] 하드웨어 발열 및 성능 최적화 검증
실시간 FFT, LiDAR 3D 렌더링 구동 중의 프레임레이트(FPS) 및 스레드 자원 점유 상태 진단 및 개선
