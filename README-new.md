# DevOps GitOps Repository

이 저장소는 Argo Project를 사용한 GitOps 기반 CI/CD 파이프라인을 구현합니다.

## 🏗️ 아키텍처

```
📁 dev-gitops/
├── apps/                           # ArgoCD Application 정의들 (App of Apps 패턴)
│   ├── go-web.yaml                 # Go 웹 애플리케이션
│   ├── argo-events-infra.yaml      # Argo Events 인프라
│   ├── argo-workflows-infra.yaml   # Argo Workflows 인프라
│   └── root-app.yaml              # 최상위 App of Apps
├── infrastructure/                 # 인프라스트럭처 리소스들
│   ├── argo-events/
│   │   ├── eventbus.yaml          # EventBus 설정
│   │   ├── github-eventsource.yaml # GitHub Webhook 설정
│   │   └── github-sensor.yaml     # GitHub 이벤트 센서
│   └── argo-workflows/
│       ├── real-docker-build-workflow.yaml # CI/CD 파이프라인
│       └── workflow-rbac.yaml     # 워크플로우 권한 설정
├── manifests/                     # 애플리케이션 매니페스트
│   └── go-web/
│       └── deploy.yaml            # Go 웹 앱 배포 설정
└── README.md
```

## 🚀 App of Apps 패턴

이 저장소는 ArgoCD의 **App of Apps** 패턴을 사용하여 다음을 달성합니다:

- ✅ **완전한 GitOps**: 모든 변경사항이 Git을 통해 관리됨
- ✅ **자동화된 배포**: Git push → 자동 빌드 → 자동 배포
- ✅ **인프라 코드화**: Argo Events, Workflows 설정도 GitOps로 관리
- ✅ **단일 진실 공급원**: Git이 모든 설정의 SSOT

## 🔄 CI/CD 플로우

1. **개발자가 go-web-source에 코드 push**
2. **GitHub Webhook이 Argo Events 호출**
3. **Argo Events가 Argo Workflows 트리거**
4. **Workflows가 이미지 빌드 & GitOps 레포 업데이트**
5. **ArgoCD가 변경사항 감지하여 자동 배포**

## 🛠️ 설정 방법

### 1. Root App 배포 (최초 1회)
```bash
kubectl apply -f apps/root-app.yaml
```

### 2. ArgoCD UI에서 확인
- `root-app`이 자동으로 다른 모든 Application들을 생성
- 모든 컴포넌트가 `Synced` 및 `Healthy` 상태인지 확인

### 3. GitHub Webhook 설정
Repository Settings → Webhooks에서:
- Payload URL: `https://<argo-events-endpoint>/push`
- Content type: `application/json`
- Events: Push events

## 🎯 이점

### 기존 방식 vs App of Apps
| 기존 방식 | App of Apps |
|-----------|-------------|
| `kubectl apply -f` 수동 배포 | Git push로 자동 배포 |
| 설정 변경 추적 어려움 | 모든 변경사항 Git 히스토리 |
| 인프라 설정 분산 관리 | 중앙집중식 GitOps 관리 |
| 환경별 설정 동기화 어려움 | 선언적 상태 관리 |

### GitOps Pull 모델
- 🔐 **보안**: 클러스터 외부에서 내부로의 접근 불필요
- 🔄 **자동화**: 클러스터가 스스로 원하는 상태 유지
- 📊 **추적성**: 모든 변경사항이 Git에 기록

## 🔧 주요 컴포넌트

- **Argo Events**: GitHub 이벤트 감지 및 워크플로우 트리거
- **Argo Workflows**: 컨테이너 이미지 빌드 및 GitOps 업데이트
- **Argo CD**: GitOps 기반 애플리케이션 배포
- **Go Web App**: 샘플 웹 애플리케이션

---

💡 **Tip**: ArgoCD UI에서 실시간으로 배포 상태를 모니터링할 수 있습니다!