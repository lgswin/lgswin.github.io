---
layout: post
title: "For Kubernetes"
date: 2025-3-27
categories: [Kubernetes]
tags: [kubernetes]]
---

## 🚀 쿠버네티스를 쉽게! Minikube, Helm, K3s, K3d, Gitea 정리

Kubernetes는 현대적인 앱 배포에 필수지만, 처음 시작하기에는 너무 복잡하게 느껴질 수 있어요.

이 글에서는 Kubernetes와 DevOps 환경을 더 쉽게 다룰 수 있게 해주는 도구들을 정리했습니다.

---

### ☁️ Minikube – 로컬 Kubernetes 연습 도구

**Minikube**는 내 컴퓨터에서 **가상의 Kubernetes 클러스터**를 실행할 수 있게 해주는 도구입니다.

실제 서버 없이 Kubernetes를 연습하거나 테스트할 수 있어요.

```bash
minikube start
```

- 설치 후 웹 대시보드: `minikube dashboard`
- 웹 앱 서비스 테스트: `minikube service [서비스명]`

🔸 **장점**

- 설치가 매우 쉽고 가볍다
- 실제 Kubernetes 환경과 거의 동일
- 다양한 드라이버(Docker, VirtualBox 등) 지원

🔸 **활용 예시**

- 쿠버네티스 명령어 연습
- 개발 중 간단한 앱을 클러스터에 배포해보기
- Helm과 연동하여 앱 배포 실습

---

### 📦 Helm – Kubernetes의 패키지 관리자

**Helm**은 Kubernetes 앱을 손쉽게 설치하고 관리할 수 있게 해주는 **패키지 관리자**입니다.

Linux에서 `apt`, `yum`, `brew`처럼 Kubernetes에는 Helm이 있어요.

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-nginx bitnami/nginx
```

- 설정 파일은 `values.yaml`로 관리
- 배포 후 수정은 `helm upgrade`, 삭제는 `helm uninstall`

🔸 **장점**

- 복잡한 YAML 설정을 템플릿으로 관리
- 앱 설정을 유연하게 바꿀 수 있음
- 대규모 앱을 한 번에 설치 가능

🔸 **활용 예시**

- Prometheus, Grafana, MongoDB, WordPress 등 설치
- CI/CD 파이프라인에서 Helm으로 자동 배포
- 공통 설정값을 여러 환경(dev/stage/prod)에서 재사용

---

### 🐳 K3s – 가볍고 빠른 Kubernetes

**K3s**는 Rancher에서 만든 **경량화된 Kubernetes** 배포판입니다.

기존 Kubernetes보다 설치가 빠르고 리소스 사용량이 적어요.

```bash
curl -sfL https://get.k3s.io | sh -
```

- 단일 실행 파일로 빠른 설치
- SQLite, etcd, MySQL 등 다양한 내장 DB 지원

🔸 **장점**

- IoT/에지 디바이스, Raspberry Pi에서도 작동
- 설치와 유지 관리가 쉬움
- 기본적으로 보안 설정이 포함됨

🔸 **활용 예시**

- 저사양 장비에 쿠버네티스 설치
- 개인 서버나 미니 PC에서 앱 운영
- 클러스터 자동화 학습 및 DevOps 실습

---

### ⚡ K3d – Docker 위에 K3s 실행

**K3d**는 Docker 컨테이너 위에 K3s 클러스터를 띄우는 도구입니다.

가볍고 빠르며, 테스트나 CI/CD 환경에서 특히 유용해요.

```bash
k3d cluster create mycluster
```

- `k3d`를 쓰면 Docker만 있으면 클러스터가 바로 실행됨
- 클러스터 삭제도 빠르고 깨끗함

🔸 **장점**

- 진짜 클러스터처럼 작동하지만 더 빠르고 가벼움
- 노드 개수도 쉽게 조절 가능 (`-agents 2`)
- CI 환경에서 자동으로 띄우고 내리기 좋음

🔸 **활용 예시**

- 빠른 테스트용 클러스터 만들기
- GitHub Actions, GitLab CI에서 쿠버네티스 배포 테스트
- 로컬 개발 환경 구성

---

### 🧑‍💻 Gitea – 자체 Git 서버 만들기

**Gitea**는 오픈 소스 **자체 호스팅 Git 서버**입니다.

GitHub처럼 Pull Request, Issue, Wiki, 사용자 관리 기능까지 지원하지만, 훨씬 가볍고 빠릅니다.

```bash
wget -O gitea https://dl.gitea.io/gitea/1.21.1/gitea-1.21.1-linux-amd64
chmod +x gitea
./gitea web
```

- 웹 브라우저로 접속 후 바로 설정 가능 (`http://localhost:3000`)
- SQLite, MySQL, PostgreSQL 등 다양한 DB 지원

🔸 **장점**

- 설치가 매우 간단 (단일 실행 파일)
- 사내/개인 Git 서버로 사용 가능
- 경량이면서도 GitHub 수준의 주요 기능 제공

🔸 **활용 예시**

- 팀 개발용 Git 저장소 운영
- 회사 내부 Git 플랫폼 구축
- CI 시스템과 연동한 코드 버전 관리

---

### 🧾 요약 비교표

| 도구 | 설명 | 설치 난이도 | 사용 용도 |
| --- | --- | --- | --- |
| **Minikube** | 로컬 쿠버네티스 | 쉬움 | 학습, 테스트 |
| **Helm** | Kubernetes 패키지 관리자 | 중간 | 앱 배포, 설정 관리 |
| **K3s** | 경량 Kubernetes | 쉬움 | IoT, 저사양 서버 |
| **K3d** | Docker 기반 K3s | 매우 쉬움 | 빠른 테스트, CI |
| **Gitea** | 자체 Git 서버 | 쉬움 | 사내 협업, 버전 관리 |