# FocusZone 기술 스택 세부 명세서
FocusZone 서비스 개발에 적용되는 네이티브 기술 및 하드웨어 가속 라이브러리의 상세 내역과 역할입니다.

## UI 및 애플리케이션 아키텍처
- **SwiftUI**
iOS 17.0 이상의 최신 선언형 UI 프레임워크를 적용하여 간결하고 가독성 높은 인터페이스를 개발합니다.
- **상태 관리 및 비동기 처리**
`@Observable` 매크로를 사용하여 메모리 내 실시간 데시벨 값 및 좌표 데이터 인스턴스를 반응형으로 관리합니다.
- **Swift 6 Concurrency 및 컴파일 요건**
동시성 안전성(Strict Concurrency Checking)을 준수하도록 설계하여, Swift 6 컴파일러 모드에서 어떠한 오류(Errors)나 경고(Warnings) 없이 정상 빌드 및 실행되는 코드로 작성합니다. 비동기 데이터 캡처 모듈(AVFoundation 등)은 `@Sendable` 및 `actor` 모델을 준수합니다.

## 3D 공간 스캐닝 및 렌더링 (AR)
- **ARKit**
LiDAR 스캐너 기반의 Scene Reconstruction 기능을 통해 벽, 바닥, 가구 등의 실내 공간 물리 구조 정보를 메쉬로 추출합니다.
- **RealityKit**
ARKit이 구성한 3D 메쉬 공간 위에 데시벨 값에 대응하는 색상 히트맵 머티리얼을 씌우고, 광선 추적(Ray-tracing) 기하 벡터를 선형 렌더링으로 가시화합니다.

## 실시간 오디오 캡처 및 주파수 분석
- **AVFoundation**
마이크 입력 스트림을 실시간 캡처하여 Raw 오디오 버퍼(PCM 데이터)를 가공 및 획득합니다.
- **Accelerate 프레임워크 (vDSP)**
하드웨어 가속 기반의 고속 푸리에 변환(FFT) 연산을 고성능으로 계산하여 소음 데시벨(dB) 데이터를 0.1초 단위의 지연 시간 없이 수집 및 분석합니다.

## 메모리 정책 (휘발성 아키텍처)
- **Memory-only 구조**
SwiftData나 CoreData와 같은 영구 스토리지 DB 설정을 차단하고, 수집 및 매핑된 모든 센서 데이터는 RAM 상에서만 보유되며 앱 세션 종료 시 자동으로 소멸(Free)되도록 설계합니다.

## 개발 검증 환경
- **Swift Testing**
XCTest 대신 현대적인 매크로 문법(`@Test`)과 파라미터화 테스트 기능(Parameterized Testing)을 사용하여 실시간 분석 모듈의 다양한 입출력 범위(dB 강도 및 신호 주파수 대역)를 체계적으로 검증합니다.
