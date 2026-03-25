# C Docker GHCR Runner

GitHub Actions와 Windows Self-hosted Runner를 활용한 C 언어 자동 빌드 및 배포 실습 프로젝트입니다.

## 프로젝트 구조
```
c-docker-ghcr-runner/
├── .github/
│   └── workflows/
│       └── c-ci-cd.yml
├── src/
│   └── main.c
├── Makefile
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
└── README.md
```

## 전체 구성도
```
[개발자 PC]
    │
    │ git push
    ▼
[GitHub Repository]
    │
    │ GitHub Actions Workflow
    ▼
[Windows Self-hosted Runner]
    ├─ Docker 이미지 빌드
    ├─ C 프로그램 테스트
    ├─ GHCR 푸시
    └─ docker compose up -d 로 운영 컨테이너 재배포
            │
            ▼
[운영 컨테이너]
    └─ C 실행 파일 포함 이미지
```

## 실습 목표

- Windows Self-hosted Runner를 GitHub Actions에 등록하고 활용
- C 소스코드를 Docker 이미지로 빌드하여 GHCR에 푸시
- Docker Compose로 운영 컨테이너 자동 재배포
- CI/CD 전체 파이프라인 구성 경험

## 사용 기술

| 항목 | 내용 |
|------|------|
| 언어 | C (C11 표준) |
| 컴파일러 | GCC |
| 빌드 도구 | Makefile |
| 컨테이너 | Docker |
| 레지스트리 | GitHub Container Registry (GHCR) |
| CI/CD | GitHub Actions |
| Runner | Windows Self-hosted Runner |

## 사전 준비

Windows Self-hosted Runner 서버에 아래가 준비되어 있어야 합니다.

- Git 설치
- Docker Desktop 또는 Docker Engine 사용 가능 상태
- GitHub 저장소와 연결된 Self-hosted Runner 등록
- PowerShell 관리자 권한

## 로컬 빌드 방법
```bash
# 빌드
make

# 실행
make run

# 정리
make clean
```

## Docker 빌드 및 실행
```bash
# 이미지 빌드
docker build -t c-calculator .

# 컨테이너 실행
docker run --rm c-calculator

# Docker Compose로 실행
docker compose up -d
```

## CI/CD 동작 방식

`main` 브랜치에 push하면 GitHub Actions가 자동으로 아래 과정을 실행합니다.

1. Windows Self-hosted Runner에서 소스코드 체크아웃
2. GHCR 로그인
3. Docker 이미지 빌드
4. 컨테이너 실행 후 결과값 검증
5. GHCR에 이미지 푸시
6. Docker Compose로 운영 컨테이너 재배포
7. 실행 중인 컨테이너 상태 확인

## 예상 실행 결과
```
C Calculator Service
a = 10
b = 20
sum = 30
```

## Actions 결과 확인

저장소 상단의 **Actions** 탭에서 워크플로 실행 기록과 각 step의 로그를 확인할 수 있습니다.