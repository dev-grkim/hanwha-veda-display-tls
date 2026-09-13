# SafeVision - 전광판 제어 및 TLS 통신 서버

> 본 리포지토리는 한화 VEDA 4기 팀 프로젝트 "SafeVision" (팀명: 대홍단감자)의 일부로,
> 5인 팀 프로젝트 중 제가 직접 설계·구현한 전광판 펌웨어/드라이버, TLS 통신 서버, 평면도 변환(OpenCV) 부분만 추출하여 정리한 리포지토리입니다.
>
> 팀 프로젝트 전체 코드: [goodaehong/daehongdan](https://github.com/goodaehong/daehongdan)

## 프로젝트 개요

SafeVision은 공장의 가스·화재를 카메라 영상과 센서로 함께 판정한 뒤, 원인에 맞는 대응(환기팬·밸브·사이렌·전광판)을 자동으로 실행하는 지능형 통합 관제 시스템입니다. Raspberry Pi 4를 중앙 서버로 사용하며, 감지된 위험 단계(경고 → 위험 → 비상)에 따라 대응 장비를 제어합니다.

## 담당 역할

| 구분 | 내용 |
|---|---|
| 전광판 펌웨어 | STM32 기반 전광판 보드(HUB75 LED 매트릭스) 펌웨어 개발 |
| 전광판 드라이버 | 라즈베리파이 ↔ 전광판 보드 간 UART 통신 프로토콜 및 리눅스 측 제어 로직 구현 |
| TLS 서버 | 관제 클라이언트(Qt, Windows)와 서버 간 보안 통신을 위한 TCP/TLS 서버 구축 (OpenSSL, 인증서 지문 고정 검증) |
| 평면도 변환 | OpenCV를 이용해 공장 평면도 이미지를 화재 위치 판정 및 대피경로 계산에 쓰이는 60×60 비트맵으로 변환하는 프로그램 개발 (`evac_map_tools`) |

## 기술 스택

- **언어**: C++17, C
- **하드웨어**: Raspberry Pi 4, STM32F4
- **통신**: TCP/TLS(OpenSSL), UART
- **영상처리**: OpenCV (평면도 → 좌표 비트맵 변환)
- **디스플레이**: HUB75 LED 매트릭스 제어
- **빌드**: CMake, STM32CubeIDE

## 폴더 구조

```
.
├── server-display/     라즈베리파이 측 전광판 제어 로직
├── server-net/         관제 클라이언트 연결 (TCP/TLS)
├── server-evac-map-tools/  평면도 → 60×60 좌표 비트맵 변환 (OpenCV)
├── drivers/
│   └── stm_uart_display/   전광판 보드 UART 통신 프로토콜
├── stm32_firmware/
│   └── display_board/      전광판 보드 펌웨어 (HUB75 제어)
└── docs/                (설계/보정 관련 참고 문서, 있는 경우)
```

## 주요 구현 내용

<!-- 아래는 채워 넣을 자리입니다. 실제로 구현하며 겪었던 문제와 해결 과정을 구체적으로 적어주세요.
예시 방향:
- TLS 서버 설계 시 인증서 지문 고정 검증을 도입한 이유와 구현 방식
- 화재/가스 원인별 전광판 표시 화면 전환 로직
- STM32 ↔ 라즈베리파이 UART 프로토콜 설계 (패킷 구조, 에러 처리 등)
- OpenCV로 평면도 이미지를 60×60 좌표 비트맵으로 변환하는 알고리즘 (전처리, 스케일링, 장애물/벽 인식 방식 등)
- 발생했던 트러블슈팅 사례와 해결 과정
-->

## 빌드 방법

```bash
# STM32 펌웨어
# STM32CubeIDE로 stm32_firmware/display_board 를 열거나 CMake로 빌드 후 ST-Link로 플래싱

# 드라이버
cd drivers/stm_uart_display
make && make dt-apply && make load
```

## 팀 정보

- **소속**: 한화 VEDA 4기
- **팀명**: 대홍단감자
- **팀 규모**: 5인
- **팀 프로젝트 전체 리포**: https://github.com/goodaehong/daehongdan
