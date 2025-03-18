# argocd

```

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo


argocd version
argocd login argocdserver
argocd cluster list
argocd app list

# helm release
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata: 
  name: guestbook
  namespace: argocd
spec: 
  destination: 
    namespace: guestbook
    server: "https://kubernetes.default.svc"
  project: default
  source: 
    path: guestbook
    repoURL: "https://github.com/mabusaa/argocd-example-apps.git"
    targetRevision: master
    helm:
      releaseName: my-release    
  syncPolicy:
    syncOptions:
      - CreateNamespace=true

# multiple source
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: redis
  namespace: argocd
spec:
  project: default
  destination:
    server: https://kubernetes.default.svc
    namespace: redis
  sources:
    - chart: redis
      repoURL: https://charts.bitnami.com/bitnami
      targetRevision: 17.10.2
    - chart: prometheus-redis-exporter
      repoURL: https://prometheus-community.github.io/helm-charts
      targetRevision: 5.3.2
  syncPolicy:
    syncOptions:
      - CreateNamespace=true
```
