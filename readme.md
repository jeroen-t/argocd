1. Install Argo CD using Helm:

```
helm install argo-cd charts/argo-cd/ --namespace argocd --create-namespace
```

default credentials:

The default username is `admin`. The initial admin secret can be found:

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Port-forward the argocd api server:

```
kubectl -n argocd port-forward svc/argo-cd-argocd-server 8080:443
```

2. [Bootstrap cluster](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) using Argo CD app of apps pattern

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

3. Sync parent app:

```bash
argocd app create apps \
    --dest-namespace argocd \
    --dest-server https://kubernetes.default.svc \
    --repo https://github.com/argoproj/argocd-example-apps.git \
    --path apps  
argocd app sync apps
```