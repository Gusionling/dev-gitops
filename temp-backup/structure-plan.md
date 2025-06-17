# 새로운 App of Apps 구조

## 목표 구조:
```
dev-gitops/
├── apps/                           # ArgoCD Application 정의들
│   ├── go-web.yaml
│   ├── argo-events-infra.yaml
│   ├── argo-workflows-infra.yaml
│   └── root-app.yaml              # App of Apps
├── infrastructure/                 # 인프라 리소스들
│   ├── argo-events/
│   │   ├── eventbus.yaml
│   │   ├── github-eventsource.yaml
│   │   └── github-sensor.yaml
│   └── argo-workflows/
│       ├── real-docker-build-workflow.yaml
│       └── workflow-rbac.yaml
├── manifests/                     # 애플리케이션 매니페스트
│   └── go-web/
│       └── deploy.yaml
└── README.md
```

## 현재 문제점:
1. apps/apps/ 중복 구조
2. apps/go-web/infrastructure/ 안에 인프라 파일들이 있음
3. 정리가 필요한 구조

## 해결 방안:
1. 현재 파일들을 새 구조로 재배치
2. Application 매니페스트 생성
3. 중복 폴더 제거
