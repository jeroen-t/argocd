1. [Bootstrap cluster](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) using Argo CD app of apps pattern

```
apps
├── Chart.yaml
├── templates
│   ├── apps.yaml
│   ├── argo-cd.yaml
│   └── grafana.yaml
└── values.yaml
```

The [templates](app/templates) folder contains one file for each child app. Adding new app files to this folder will automatically create and sync the child app.

2. Sync parent app:

```bash
argocd app create apps \
    --dest-namespace argocd \
    --dest-server https://kubernetes.default.svc \
    --repo https://github.com/argoproj/argocd-example-apps.git \
    --path apps  
argocd app sync apps
```