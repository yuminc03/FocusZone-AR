# FocusZone 구현 전 사전 지식 가이드
FocusZone 구현을 진행하기 전에 학습하거나 참고해야 할 핵심 개념과 Apple 공식 문서 레퍼런스 모음입니다.

## 1. 선형대수학 기하학 연산 (소리 반사 벡터)
소리가 벽면 메쉬에 부딪혀 반사되어 나가는 경로를 3D 선으로 시각화하기 위해 광선 추적(Ray-tracing) 기하 연산이 필요합니다.
- **벡터 내적 (Dot Product)**
두 3D 벡터 $\mathbf{a}$와 $\mathbf{b}$의 내적 $\mathbf{a} \cdot \mathbf{b}$는 방향의 일치도 및 한 벡터에서 다른 벡터로의 투영 길이를 나타내며, 법선 벡터를 기준으로 소리가 들어오는 정도를 계산하는 데 필수적입니다.
- **반사 벡터 공식**
입사 진행 방향 벡터를 $\mathbf{d}$, 충돌한 벽면 메쉬의 법선 벡터(Unit Normal Vector)를 $\mathbf{n}$이라 할 때, 반사 각도 벡터 $\mathbf{r}$은 아래 공식을 통해 연산됩니다.
$$\mathbf{r} = \mathbf{d} - 2(\mathbf{d} \cdot \mathbf{n})\mathbf{n}$$
이 계산은 RealityKit 렌더링 루프에서 실시간으로 프레임마다 연산되므로 SIMD(Single Instruction Multiple Data) 형태의 `SIMD3<Float>` 타입을 활용하여 최적화해야 합니다.

## 2. ARKit & RealityKit (3D 공간 매핑)
LiDAR 스캐너를 장착한 iOS 기기에서 주변 환경의 형상 정보를 가져오는 프레임워크입니다.
- **핵심 개념**
  - **Scene Reconstruction**: LiDAR 데이터를 종합하여 방 전체를 실시간 다각형 3D 메쉬로 생성합니다.
  - **ARMeshAnchor**: 스캔된 공간을 일정한 청크 단위의 격자로 분할하여 지오메트리 vertices(정점) 및 indices(인덱스) 정보를 제공합니다.
- **공식 레퍼런스 문서**
  - [ARWorldTrackingConfiguration.SceneReconstruction API 문서](https://developer.apple.com/documentation/arkit/arworldtrackingconfiguration/scenereconstruction) (iOS LiDAR 기반 3D 메쉬 생성 핵심 설정 속성)
  - [ARMeshAnchor API 문서](https://developer.apple.com/documentation/arkit/armeshanchor)

## 3. AVFoundation & Accelerate (실시간 주파수 분석)
마이크를 통해 하드웨어 수준에서 들어오는 음원을 데시벨(dB) 및 주파수 대역별 강도로 전환하는 역할을 수행합니다.
- **핵심 개념**
  - **Audio Engine (AVAudioEngine)**: 마이크 입력 노드(Input Node)에 Tap을 설치하여 PCM Raw 오디오 버퍼를 일정 프레임 크기(예: 1024 또는 2048 샘플)로 계속 추출합니다.
  - **vDSP (Digital Signal Processing)**: Accelerate 라이브러리의 하드웨어 가속 연산 모듈입니다.
  - **FFT (Fast Fourier Transformation)**: 시간축 기반의 오디오 신호를 주파수 대역별 진폭 신호로 변환하여 소음 수준을 정확히 계측합니다.
- **공식 레퍼런스 문서**
  - [Accelerate vDSP.FFT 공식 문서](https://developer.apple.com/documentation/accelerate/vdsp/fft)
  - [vDSP Fourier Transforms 개념 가이드](https://developer.apple.com/documentation/accelerate/vdsp/fast_fourier_transforms)

## 4. Swift 6 Concurrency (비동기 안전성)
마이크 센서 스레드와 AR 렌더링 메인 스레드 간에 실시간 대용량 데이터가 오가므로 동시성 안전성이 매우 중요합니다.
- **핵심 개념**
  - **Actor**: 상태 변경을 단일 스레드 내에서만 동기적으로 처리하여 데이터 레이스(Data Race)를 원천적으로 방지합니다.
  - **@Sendable**: 서로 다른 격리된 도메인(스레드) 간에 안전하게 전달될 수 있는 값 타입 혹은 참조 타입임을 보장하는 특성입니다.
