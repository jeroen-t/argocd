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

Login to argocd through cli:

```
argocd login localhost:8080
```

2. [Bootstrap cluster](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/) using Argo CD app of apps pattern

```
apps
├── templates
│   ├── applications.yaml
│   └── projects.yaml
├── Chart.yaml
├── values-dev.yaml
├── values-prd.yaml
├── values-stg.yaml
└── values.yaml
```

3. Sync parent app using environment file (see last arg):

```bash
argocd app create apps \
    --dest-namespace argocd \
    --dest-server https://kubernetes.default.svc \
    --repo https://github.com/jeroen-t/argocd.git \
    --path apps \
    --values values-dev.yaml
argocd app sync apps
```